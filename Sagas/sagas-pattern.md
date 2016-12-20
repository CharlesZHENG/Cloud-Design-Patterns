# Sagas

Sagas����һ���������ģʽ��Ҳͬʱ���ڿ��Ƹ��������ִ�кͻع��ȡ�

Sagas���ʼ�ĳ�������ΪһЩ��ʱ��������ʵ�֣��ʼ��ʱ���������Ϊ�����ڵ����񣩣�����Ҳ����һЩ��Խ�������ķֲ�ʽ������Щ��ʱ������������޷��򵥵�ͨ��һЩ���͵�ACIDģ��ʹ�ö���ύ��ϳ������ķ�ʽ��ʵ�֡�Sagas������ʽ�������������⣬�Ͷ��ʽ����ͬ��Sagas�Ὣ�����ֳɵ��������񣬰��������Ĳ����ͻع��Ĳ�����
����ͼ��

![](Sagas.png)

��ͼչʾ��һ���򵥵�Saga��������ÿ�Ԥ�����г̣���ҪԤ�������������Լ��õꡣ����޷���ȡȫ������Ϣ��������þ��ǲ�Ҫ�������Կ�������˵���϶��޷������еķ��񶼶���Ϊ�ֲ�ʽ��ACID������ʱ���Խ��⳵��Ϊ����Ϊһ�����壬���а������ȥԤ���Լ����ȡ������Ȼ����Ʊ�;Ƶ�Ҳ�ṩͬ���ķ���

Ȼ����Щ��Ϊ���������һ�𹹳���һ����Ϊ���������߻����Խ�������Ϊ�����ܣ�����ֻ�и���Ϊ���Ľ����߲��ܹ��ٿ������Ϊ������һ����Ϊ��ɺ󣬻Ὣ��ɵ���Ϣ��¼��һ�����ϣ�����˵����һ�����У��У�֮�����ͨ��������Ϸ��ʵ���Ӧ����Ϊ����һ����Ϊʧ�ܵ�ʵ�գ���Ϊ������������ϣ�Ȼ����Ϣ���͸��ü��ϣ��Ӷ�·�ɵ�֮ǰִ�гɹ�����Ϊ��Ȼ��ع����е�����

��������߶��������г��˽�һЩ�Ļ����ͻ�֪���������Ϊ����ʵ���з��յġ�һ����˵����ǰԤ���⳵���񼸺�����ɹ�����Ϊ�⳵��˾�������㹻��ʱ���������㰲�ų���������Ԥ�����ݾ���һЩ�����ˣ�һ����Ѻ�������£�ֻ����ǰ24СʱԤ������������˸�һ������»�Ҫ�շѵģ��������Ԥ���ǶԵġ�

## ʹ�þ���

����ĳ�����Ϊ�������԰������Ǹ��õ��˽�Sagas����

���������һ�����͵ļ����������ʶ�Ӧ����Ϊ���е���Ϊ���ᴴ��3�������Ľ��̣�ÿһ�����̶��Ḻ��һ��ָ�������񡣷ֱ����⳵��Ԥ���Ƶ��Լ�Ԥ����Ʊ��������������

```
static ActivityHost[] processes;

static void Main(string[] args)
{
    var routingSlip = new RoutingSlip(new WorkItem[]
    {
        new WorkItem<ReserveCarActivity>(new WorkItemArguments),
        new WorkItem<ReserveHotelActivity>(new WorkItemArguments),
        new WorkItem<ReserveFlightActivity>(new WorkItemArguments)
    });

    // imagine these being completely separate processes with queues between them
    processes = new ActivityHost[]
    {
        new ActivityHost<ReserveCarActivity>(Send),
        new ActivityHost<ReserveHotelActivity>(Send),
        new ActivityHost<ReserveFlightActivity>(Send)
    };

    // hand off to the first address
    Send(routingSlip.ProgressUri, routingSlip);
}

static void Send(Uri uri, RoutingSlip routingSlip)
{
    // this is effectively the network dispatch
    foreach (var process in processes)
    {
        if (process.AcceptMessage(uri, routingSlip))
        {
            break;
        }
    }
}
```

���е�`AcitivityHost`���Ƕ��ⲿ�����һ������`RoutingSlip`�Ƕ�ǰ��˵�ļ��ϵĳ���
�����Ǿ����һ������ļ�ʵ�֣�`ReserveHotelActivity`�Լ�`ReserveFlightActivity`��ʵ�־Ͳ��ڴ˴��г��ˣ�������`ReserveCarActivity`��ʵ�֡�������Ҫ�����ļ���������

`DoWork`�Լ�`Compensate`������Activity�������������ִ��ʵ�ʲ����Լ��ع��Ĳ���������
`WorkItemQueueAddress`�Լ�`CompensationQueueAddress`����������������Ӧ����ġ��ο����´��룺

```
class ReserveCarActivity : Activity
{
    static Random rnd = new Random(2);

    public override WorkLog DoWork(WorkItem workItem)
    {
        Console.WriteLine("Reserving car");
        var car = workItem.Arguments["vehicleType"];
        var reservationId = rnd.Next(100000);
        Console.WriteLine("Reserved car {0}", reservationId);
        return new WorkLog(this, new WorkResult
        {
            { "reservationId", reservationId }
        });
    }

    public override bool Compensate(WorkLog item, RoutingSlip routingSlip)
    {
        var reservationId = item.Result["reservationId"];
        Console.WriteLine("Cancelled car {0}", reservationId);
        return true;
    }

    public override Uri WorkItemQueueAddress
    {
        get { return new Uri("sb://./carReservations"); }
    }

    public override Uri CompensationQueueAddress
    {
        get { return new Uri("sb://./carCancellactions"); }
    }
}
```

`RoutingSlip`�ǶԳɹ���Ϊ���ϵĳ���������������Ӧ�ķ��񣬰������������У�һ������ɵ�����һ���ǵȴ�ִ�е�����`RoutingSlip`��Ҫ�����������Ӷ����Ϊ������ɹ��ͻὫ������ǰִ�У����ʧ�ܾͻ����ִ��
��`RoutingSlip`ʹ�ö�������ǰִ�У�ʹ��ջ�����ִ�С�
```
class RoutingSlip
{
    readonly Stack<WorkLog> completedWorkLogs = new Stack<WorkLog>();
    readonly Queue<WorkItem> nextWorkItem = new Queue<WorkItem>();

    public RoutingSlip()
    {
    }

    public RoutingSlip(IEnumerable<WorkItem> workItems)
    {
        foreach (var workItem in workItems)
        {
            this.nextWorkItem.Enqueue(workItem);
        }
    }

    public bool IsCompleted
    {
        get { return this.nextWorkItem.Count == 0; }
    }

    public bool IsInProgress
    {
        get { return this.completedWorkLogs.Count > 0; }
    }

    public bool ProcessNext()
    {
        if (this.IsCompleted)
        {
            throw new InvalidOperationException();
        }

        var currentItem = this.nextWorkItem.Dequeue();
        var activity = (Activity) Activator.CreateInstance(
            currentItem.ActivityType);

        try
        {
            var result = activity.DoWork(currentItem);
            if (result != null)
            {
                this.completedWorkLogs.Push(result);
                return true;
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Exception {0}", e.Message);
        }
        return false;
    }

    public Uri ProgressUri
    {
        get
        {
            if (IsCompleted)
            {
                return null;
            }
            else
            {
                return
                    ((Activity)Activator.CreateInstance(this.nextWorkItem.Peek().ActivityType)).
                        WorkItemQueueAddress;
            }
        }
    }

    public Uri CompensationUri
    {
        get
        {
            if (!IsInProgress)
            {
                return null;
            }
            else
            {
                return ((Activity) Activator.CreateInstance(
                    this.completedWorkLogs.Peek().ActivityType)).
                        CompensationQueueAddress;
            }
        }
    }

    public bool UndoLast()
    {
        if (!this.IsInProgress)
        {
            throw new InvalidOperationException();
        }

        var currentItem = this.completedWorkLogs.Pop();
        var activity = (Activity) Activator.CreateInstance(
            currentItem.ActivityType);

        try
        {
            return activity.Compensate(currentItem, this);
        }
        catch (Exception e)
        {
            Console.WriteLine("Exception {0}", e.Message);
            throw;
        }
    }
}
```
`ActivityHost`�����`RoutingSlip`�����`ProcessNext`������������һ����Ϊ���������`DoWork()`���߷������`Compensate()`����.

```
abstract class ActivityHost
{
    Action<Uri, RoutingSlip> send;

    public ActivityHost(Action<Uri, RoutingSlip> send)
    {
        this.send = send;
    }

    public void ProcessForwardMessage(RoutingSlip routingSlip)
    {
        if (!routingSlip.IsCompleted)
        {
            // if the current step is successful, proceed
            // otherwise go to the Unwind path
            if (routingSlip.ProcessNext())
            {
                // recursion stands for passing context via message
                // the routing slip can be fully serialized and passed
                // between systems.
                this.send(routingSlip.ProgressUri, routingSlip);
            }
            else
            {
                // pass message to unwind message route
                this.send(routingSlip.CompensationUri, routingSlip);
            }
        }
    }

    public void ProcessBackwardMessage(RoutingSlip routingSlip)
    {
        if (routingSlip.IsInProgress)
        {
            // UndoLast can put new work on the routing slip
            // and return false to go back on the forward
            // path
            if (routingSlip.UndoLast())
            {
                // recursion stands for passing context via message
                // the routing slip can be fully serialized and passed
                // between systems
                this.send(routingSlip.CompensationUri, routingSlip);
            }
            else
            {
                this.send(routingSlip.ProgressUri, routingSlip);
            }
        }
    }

    public abstract bool AcceptMessage(Uri uri, RoutingSlip routingSlip);
}
```

## ��ص�����ģʽ

* **Data Consistency Primer** Compensating-Transactionͨ���������ع���Ҫʵ������һ����ģ�͵Ĺ��ܡ�**Data Consistency Primer**�е������ṩ�˸����������һ���Ե����Ե�˵����

