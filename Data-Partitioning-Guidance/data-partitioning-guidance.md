# Data Partitioning Guidance

在很多大规模的解决方案中，数据都是分成单独的分区，可以分别进行管理和访问的。而分割数据的策略必须仔细的斟酌才能够最大限度的提高效益，同时最大限度的减少不利影响。数据的分区可以极大的提升可扩展性，降低争用以及优化性能。

## 为何进行数据分区

绝大多数的云应用和云服务都会将存储和检索数据作为其业务的一部分。而应用所使用的数据仓库的设计对于系统的性能，吞吐，扩展性都有着非常重要的影响。其中在大规模系统中常用的一种技术，就是对数据进行分区。

> 本文中所提到的数据分区，指的是物理上将数据分割成不同的数据仓库。这并不同于SQL Server 表的拆分，这两者完全是两种不同的概念。

对数据进行分区包含了很多的好处，如下：

* 分区可以提升扩展性，仅仅使用单一数据库系统迟早会触及物理硬件的极限的，而对数据进行多个分区（使用不同的Host）的访问，理论上，是允许系统无限地扩展的。
* 分区可以提升性能。数据访问操作每次只是访问一小部分数据，却会独占IO，当对数据进行了分区，数据的访问就被分割到不同的Host上面了，性能会极大的提高。获取数据的信息可以并行的进行。并且，每个分区可以在地里位置上距离对应的应用服务器很近，极大的降低网络延时对性能所产生的影响。
* 分区可以提升可用性。将数据分割成多个不同的部分可降低单端点的错误对整体的影响。如果一个分区的服务挂掉了，其它服务的数据请求仍然是可以继续的，只有挂掉分区中的数据是不可用的。针对于其他数据的服务仍然是正常的。增加分区的数量可以减少单机错误对服务整体可用性的影响。而对每个数据分区进行复制可以进一步降低单个分区错误所造成的影响。同时，数据分区可以将必须持续保证高可用的关键数据和一些低值数据（比如日志文件）分离。
* 分区可以增强安全性。根据数据的一些特性，以及分区的特点，可以将敏感数据和非敏感数据分割到不同的分区。之后，针对不同的服务器和数据仓库进行安全性处理和优化。
* 分区可以提供操作性弹性扩展。数据分区为很多操作的适配提供更多的选择，可以最大化管理效率，减少消耗。不同的分区可以定义不同的策略来管理，监控，备份和恢复，以及针对数据重要性的不同而进行的管理任务。
* 分区可以使用合适的数据仓库来匹配其使用场景。数据分区可以让不同的存储使用不同的类型的数据仓库，完全可以根据数据仓库的特性来进行选择。例如，大的二进制数据可以存储在blob数据仓库，而结构化数据可以存储在文档数据库。想了解更多的信息，可以参考MSDN上面的文章**[Using SQL, NoSQL, and Polyglot Persistence on MSDN](http://msdn.microsoft.com/en-us/library/dn271399.aspx).**

## 设计分区

数据分区可以采用不同的方式：横向，竖向，根据功能这三种。开发者可以根据其使用场景和需求选择合适的策略来进行数据分区。

> 本文中描述的数据分区的体系和数据仓库使用的一些技术是彼此独立的。数据分区完全可以使用不同的数据仓库，包括关系型数据库和NoSQL类型的数据库。

## 分区策略

常见的三种分区策略如下：

* 横向分区（通常称作sharding）,在这个策略中，每个分区本身都是一个数据存储区，但所有分区都有相同的模式。每个分区被称为碎片并持有一个特定的数据集，例如在一个电子商务应用一组特定客户的订单。
* 纵向分区。该策略中，每个分区拥有数据仓库的某些部分数据域（举例来说就是一个数据的某些表分发到不同的数据库上面）。这些数据域可以根据其使用的场景来进行分区。比如说，将经常访问的一些数据库置于一个数据仓库，而不常访问的数据仓库置于另一个数据仓库。
* 功能性分区。该策略中，数据是根据其在系统中如何使用或者聚合的方式来进行分区的。例如，在一个电商系统中，为发票和管理产品的库存实现了业务分离功能，可能将发票数据置于一个分区，而产品库存信息置于另一个分区。

需要注意的是，以上描述的三种不同的策略是可以联合使用的。他们并不是互斥的，开发者可以在考虑数据分区的时候同时考虑。举例来说，开发者可以将数据区分成不同的shard然后使用纵向分区策略来将每个shard中的内容分区。类似地，可以通过功能性分区来讲shard中的数据继续分区等。

然后，每个策略不同的需求也会引起一系列的冲突问题，开发者必须在这设计分区的时候进行权衡，才能满足系统对数据处理的性能要求。下面将针对每个策略的细节进行具体的描述。

### 横向分区（Sharding）

图1展示了横向分区(sharding)的概览图。在这个例子当中，产品库存数据基于产品的key被区分成不同的shard(每个shard包含了连续返回的shard key，以字母顺序管理)

![](leanote://file/getImage?fileId=58943b47fcd5766ac2000004)

图1.横向分区(sharding)，基于key来分割数据

Sharding可以让应用承载更多的计算机，减少争用，以及提升性能。开发者可以轻易的通过增加shard和服务器来扩展系统。

实现横向分区策略最关键的一个点就是sharding的key。在系统运行后，想要更改key就很困难了。Key必须保证数据在正确的分区，并且足够的平均，这样才能令工作负载平摊。同时，确保单个shard的访问没有超过其数据仓库能承受的性能上限也非常重要。

同时，shard的结构也避免创建hotspot(过度访问的分区)，因为那样可能影响性能以及可用性。举个例子，使用基于客户Id的哈希而非客户名字的首字母进行分片，可以有效防止因为字母频率不同所引起的不平衡。这是一种典型的用来帮助数据进行平均分区的技术。

开发者选择的sharding key应该尽量减小将来继续分片，将一些分片合并成大分区或者修改数据仓库的描述等需求。这些操作将非常耗时，并且可能会需要很多分片临时下线才能完成这些操作。如果shard复制了，当其他分片在执行分片，合并或者重新配置的时候，尽量保证一些拷贝是在线的，当进行这些处理的时候，系统可能需要限制对这些被操作的分片的访问。例如，处于拷贝中的数据可能需要被标记为只读，以限制写操作所带来的不一致性。

> 想了解更多的关于实现Sharding和横向分区的实践技术和所需要考虑的问题，可以参考[Sharding模式](../Sharding/sharding-pattern.md)来了解更多信息。

### 纵向分区

使用纵向分区最常见的场景就是为了减少由应用最频繁访问数据的条目数。图2展示了纵向分区的一个概览图，数据的不同的属性处于不同的分区之中。


![](leanote://file/getImage?fileId=58943e08fcd5766ac2000005)

图2.纵向分区根据其场景来组织数据

在上面的例子中，应用最频繁请求的是产品的名字以及描述和价格信息，并将这些信息展示给客户。而库存级别以及产品上一次从厂家预定的日期就被放在了不同的分区，因为通常情况下，这两种数据是一起使用的。这种情况下进行的数据分区会有一个额外的优势，就是相对来说，变化较少的数据（比如产品名字，描述，以及价格等）数据和很容易和一些动态数据（库存，预定日期）等分离开来。应用可以选择性的去缓存那些很少改变的数据到内存中来最大化性能。

另一种典型的应用场景就是为了分离敏感数据，来增加对敏感数据的保护。举个例子，可以将信用卡的卡号和其对应的安全验证码分离到不同的分区中。

垂直分区还可以减少数据所需的并发访问量。

> 垂直分区在数据存储区内的实体级别上运行，部分地将包含多个字段的实体规范化为多个实体，并用更少的字段。

### 功能分区

有些系统可以为每个不同的业务领域或服务的应用程序识别有界的上下文，功能分区为这类系统的隔离和数据访问性能的提高提供了技术支持。图3展示了功能分区的一个概览图，其中库存数据和客户数据是进行了分离的。


![](leanote://file/getImage?fileId=58944120fcd5766ac2000006)

图3 功能性分区通过上下文和子域来进行数据分区

功能分区策略可以帮助减少系统不同部分之间的访问冲突。

### 为扩展性设计分区

考虑每个分区的负载和大小，并且平衡他们是必不可少的，只有这样才能最大化扩展性。同时，开发者必须对数据进行分区，使其不超过单个分区存储区的物理限制。

在考虑设计分区扩展性的时候，可以遵循如下的一些步骤：

1. 分析应用来理解数据的访问模式，每次的请求的大小，访问频率以及内在的等待时间，和一些计算处理需求比如存储过程。在很多情况下，都是少量实体需要大量的处理资源。
2. 基于对应用的分析，判断当前以及未来的规模目标。比如数据大小以及工作负载，并且对数据进行分区来达到对应的规模目标。在横向分配策略中，选择合适的shard key是非常重要的，只有这样才能将数据尽可能均等的分区。关于更多的信息，可以参考[Sharding模式](../Sharding/sharding-pattern.md).
3. 分区的时候需要确保每个分区所需要的资源足够，能够在吞吐和大小上达到需求值。例如，某个分区的节点在存储空间，处理能力，网络带宽上都存在限制。如果数据存储和处理需求很容易就超过这些限制的话，重新选择新的分区策略或者分割数据就是十分必要的了。比如，一个扩展的方法可能会通过使用分离数据仓库的方式来分离核心应用的日志数据和应用特性相关的数据来防止服务器上数据存储超过了单个节点的容量上限。如果数据仓库的总数超过了节点限制，使用分离的数据节点就十分必要了。
4. 监控使用中的系统来验证数据如预期的那样工作，并且分区可以处理相应的负载。有可能分区的使用量和分析的并不是十分的一致的，如果需要的话，可以针对系统部分不合适的地方进行重新设计来尽可能的平衡各个分区的负载以达到性能和扩展性的需求。

需要注意的是，一些云环境中分配资源的时候会涉及到基础设施的限制，开发者应该确保自己选择的限制能够预留足够的空间以应对数据量的增长，包括磁盘空间，处理能力，带宽等。举个例子，如果开发者使用Windows Azure表存储，频繁的shard可能需要更多的资源来处理一个单表的请求（单表在指定时间内处理请求的数量是存在限制的）。在这种情况下，分片可能需要分割成多个表来分散负载。如果这些表的总大小超过了一个存储账户的容量，就需要创建额外的存储账户来分散负载。如果账户的数量超过了一个subscription中的账户上限，就需要使用多个subscription了。

### 为请求性能设计分区

通常可以通过使用小的数据集合和并行请求执行来增加查询性能。每个分区都包含全部数据的一部分，这样可以有效提高查询的性能。然而，分区并不是提高性能的唯一选择，还要看数据库是否配置正确。比如说，在使用关系型数据库的时候，是否配置了必要的索引来提高性能。

在为了提高查询性能而设计分区的时候可以参考如下的一些步骤：

1. 分析应用来识别出：
 - 那些执行缓慢的请求。
 - 那些关键的而且必须快速执行的请求。
2. 将性能缓慢的数据进行分区。必须确保如下方面：
 - 限制每个分区的大小，这样请求响应时间才能达到目标。
 - 在实现横向分区的时候，设计shard key的计算方式，让应用可以轻易找到正确的分区。这样可以防止请求扫描全部分区。
 - 考虑一个分区的位置对查询性能的影响。如果可能的话，尝试将数据保存在地理上接近应用程序和访问它的用户的分区中。
3. 如果实体有吞吐和请求性能需求，使用基于实体的功能性分区。如果仍然无法满足需求，也应用横向分区来解决问题。在绝大多数场景下，单独使用一种分区策略就足够了，但是在某些场景下，需要考虑多种策略配合使用来进行分区。
5. 跨分区的查询请求可以通过并行查询来增强性能。

### 为可用性设计分区

分区数据通过确保整个数据集不会因为一个端点的错误而影响全部的业务以及独立管理每个分区的数据集来提升应用的可用性。当设计和实现分区时，可以考虑如下影响可用性的因素：

* 如何管理分区。设计分区，以支持独立的管理和维护提供了几个优点。例如:。
 - 如果某个分区失败了，可以在不影响应用实例的情况下，直接访问其他的分区。
 - 不同地域的数据支持配置定时管控任务，可以在非高峰时期进行数据的同步工作。确保分区不太大，以防止任何计划的维护完成在此期间。
* 业务操作是如何使用关键数据的。 有些数据是包含一些敏感的业务信息的，比如发票细节或者银行事务等。其他的数据可能就并非业务相关的数据了，比如说日志文件，性能分析等。在判断了数据的类型之后，可以考虑：
 - 将关键的数据放在高度可靠的分区，并且有一些备份的方案。
 - 对于不同重要性的数据集合，建立不同的监控和管理机制。将相同重要级别的数据放到同一个分区，这样可以以更为合适的频率来对分区的数据进行备份操作。举个例子，那些存放了银行事务的分区的备份频率显然应该高于那些存放了日志数据的分区。
* 考虑是否跨分区复制关键数据。这样的策略可以提供可用性和性能，当然了，也会引入一些一致性的问题。同步不同分区中的数据是需要不少时间的，在同步的这端时间内，就会出现不一样的数据值了。

## 问题和顾虑

在设计数据分区的时候会有如下的一些顾虑：

* 如果可以，尽可能的将最常用、通用的数据库操作的数据放到一个分区当中，来尽可能的减少跨分区的数据访问操作。跨分区的查询数据操作会比访问一个分区要消耗更多的时间。在跨分区查询不可避免的场景下，为了最小化跨分区的查询时间，就只通过在应用内使用并行查询请求，并且在聚合返回的结果来优化查询时间了。然而，这种方法在有的时候也是不可用的，比如不同的分区查询结果存在相互依赖的情况下（需要先从一个分区获取数据作为下一个分区的查询条件）。
* 如果查询使用相对变动很小的数据，比如邮政编码表或者产品列表等，可以考虑所有的分区使用完全复制的分区策略，来减少寻找不同分区的时间。
* 在可能的情况下，最小化对垂直和功能分区的引用完整性的要求。在这些方案中，应用程序本身负责在数据更新和消耗时保证跨分区的引用完整性。查询请求必须要跨越多个分区来结合数据，这会比在同一个分区结合数据要慢的多。因为应用程序通常需要根据键和外键进行连续查询。相反，考虑复制或取消相关数据的规范化。若要最小化跨分区连接所需的查询时间，请在分区上执行并行查询，并在应用程序中进行数据的结合操作。
* 考虑分区方案可能对分区之间数据一致性的影响。开发者应该评估强一致性是否是一个必要的需求。相反，云中的一种常见方法是实现最终一致性。每个分区中的数据分别更新，并且应用程序逻辑可以负责确保所有的更新都能成功完成，以及处理最终一致的操作正在运行时查询产生的数据不一致。有关实现最终一致性的更多信息，请参见[Data Consistency Primer](../Data-Consistency-Primer/data-partitioning-guidance.md)。
* 考虑查询如何定位数据所在的分区。如果查询必须扫描所有分区来定位所需的数据，即使使用多个并行查询，也将显著的影响性能。使用垂直和功能分区策略的查询可以自然指定分区。然而，在使用水平分区（sharding）的时候，定位一个元素是很困难的因为每个断片都有相同的架构。典型sharding的解决方案是保持映射，令映射可以用来查找数据项的shard的位置。该映射可以在分片逻辑的应用实施，或由数据存储是否支持透明的碎片。
* 使用水平分区策略时，需要考虑定期重新平衡碎片分发数据均匀的规模和工作量减少某个分区的过度访问，最大限度地提高查询性能以及减少物理存储的局限性影响。当然，这是一个较为复杂的任务，通常需要使用自定义工具或方法来实现。
* 复制每个分区为失败操作提供额外的保护。如果单个副本失败，则可以将查询指向其他备份的工作副本。
* 如果达到分区策略的物理限制，则可能需要将可扩展性扩展到不同级别。例如，如果分区位于数据库级别，则可能意味着在多个数据库中定位或复制分区。如果分区已经在数据库级别，物理限制是一个问题，它可能意味着在多个托管帐户中定位或复制分区。
* 避免在多个分区中访问数据的事务。某些数据存储会保证数据进行修改的操作的事务一致性和完整性，但仅当它位于单个分区中时。如果需要跨多个分区进行事务性支持，则可能需要将此作为应用程序逻辑的一部分才可以实现，因为大多数分区系统不提供对于跨分区事务的支持。

所有数据存储需要一些操作管理和监视活动。任务的范围可以从加载数据，备份和恢复数据，整理数据，并确保系统执行的正确性和有效性。

考虑影响运营管理的以下因素：

* 考虑执行一个定期任务来定位任何数据完整性问题，或尝试自动修复这些问题或发出警报。
* 考虑如何在数据被分区时执行适当的管理和操作任务，如备份和还原、归档数据、监视系统以及其他管理任务。例如，在备份和还原操作中保持逻辑一致性在实现上就是一个挑战。
* 考虑如何将数据加载到多个分区，以及新的数据如何从其他源插入到不同分区中。一些工具和实用程序可能不支持分片的数据操作，如数据加载到正确的分区，所以这些操作可能需要定制新的工具和实用程序来完成。
* 考虑如何将数据定期存档和删除（也许每月），以防止分区数据的过度增长。有可能还需要转换数据以匹配不同的归档模式。

## 相关的模式

在应用程序和数据存储中实现数据分区时，可以考虑参考如下的一些模式或者方案：

* **[Sharding模式](../Sharding/sharding-pattern.md)**.Sharding模式可以通过新增分布的存储节点来实现数据存储规的扩展。此模式描述如何将数据存储分割为水平分区。
* **[Data Consistency Primer](../Data-Consistency-Primer/data-consistency-primer.md)**.管理和维护跨分区的数据一致性是一个重要的问题，特别是在并发性和可用性可能出现问题。开发者经常需要使用最终一致性模型来而非使用强一致性。[Data Consistency Primer](../Data-Consistency-Primer/data-consistency-primer.md)讨论了两种一致性模型优点和局限性的。
* **[Data Replication and Synchronization Guidance](../Data-Replication-and-Synchronization-Guidance/drasg.md)**. 数据可以跨分区在不同的位置复制，并且可能需要对副本进行周期性地同步，以确保所有分区的数据一致。[Data Replication and Synchronization Guidance](../Data-Replication-and-Synchronization-Guidance/drasg.md)总结了分布在多个位置的复制数据的相关问题，并描述了解决这些问题的解决方案。
* **[Index-Table模式](../Index-Table/it-pattern.md)**.该模式描述了如何进行创建索引，以便在分区数据存储区中实现数据的快速检索。
* **[Materialized-View模式](../Materialized-View/mvp.md)**. 此模式描述如何生成预填充视图，以便汇总数据以支持快速查询操作。如果需要聚合或者统计的数据分布在不同的分区中的时候，可以考虑使用该模式。
