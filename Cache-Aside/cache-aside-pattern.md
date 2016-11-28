# Cache-Aside
��ģʽ�Ǵ����ݲֿ��н����ݼ��ص������У��Ӷ���߷����ٶȵ�һ��ģʽ����ģʽ������Ч��������ܣ�ͬʱҲ��һ���̶��ϱ�֤�����е����ݺ����ݲֿ��е����ݵ�һ���ԣ���ͬ�����ݵ����ݲֿ��С�

## ����

Ӧ��ͨ���������Ż�������ݲֿ���ظ����ʡ���Ȼ������е�������Զ�����ݲֿ������ݱ���һ���ǲ���ʵ�ʵġ�Ӧ���еĻ���Ӧ������һЩ�������������»��汣֤���ݵ�һ�£���Ȼ��Ҳ��Ҫ������ݹ��ڵ����������һ���Ĵ���

## �������

�ܶ���ҵ���Ļ���ϵͳ���ṩread-through��write-through/write-behind�Ĳ���������Щϵͳ��Ӧ�ôӻ���������л�����ݡ�������ݲ��ٻ����У�����͸�����浽���ݲֿ�����ȡ���ݣ�Ȼ������д�뵽�����С��κζԻ����е����ݵ��޸ģ���֮��Ҳ��д�ᵽ���ݲֿ�֮�С�

������Щ���ṩ������ܵĻ���ϵͳ�����ֵ�Ӧ�����Լ������ݱ����ڻ���֮���ˡ�
Ӧ�ÿ���ͨ��ʵ��Cache-Aside������ʵ��read-through�Ĺ��ܡ�������Ի������Ҫ�������ݲֿ��ȡ���ݡ���ͼ����Cache-Asideģʽ�Ļ������з�ʽ��

![](cache-aside-pattern.png)

1. �ж϶�ȡ����Ŀ�Ƿ��ڻ�����
2. �����Ŀ���ٻ����У������ݲֿ��н����ݶ�����
3. ���µ���Ŀд�뻺��

���Ӧ�ò��Ƕ�ȡ��Ϣ���Ǹ�����Ϣ��������ģ��write-through�Ĳ��ԣ�

1. ����Ϣ�ĸ���ͬ�������ݲֿ�
2. ����й����Ĺ�������ʧЧ

�������������һ����Ҫ��ʱ��ʹ��Cache-Asideģʽ�����ڻ�ȡ���ݵ�ʱ��ͬʱ�����ݲֿ��л�ȡ���ݣ�����д��Cache֮�С�

## ��Ҫ���ǵ�����

����ʵ������ģʽ��ʱ����Ҫ����һЩ���⣺

1. **�������ݵ�����ʱ��**���ܶ�Cacheʵ���˹��ڵĲ��Եģ���Щ���ڵĲ��Կ���ʵ�����ݵĸ��£���������ʧЧ����ͬ��Ҳ��һ��ʱ��û�з��ʵ�����ʧЧ��Ϊ����Cache-Asideģʽ�ܹ���Ч�������߱���ȷ�����ڲ����ܹ���ȷƥ��Ӧ�������ʵ����ݡ�ͬʱע�ⲻ���ù���ʱ��̫�̣���Ϊ̫�̵Ĺ���ʱ�����Ӧ��Ƶ���Ĵ����ݲֿ�������ȡ��������ӵ�Cache֮�С���Ȼ��Ҳ��Ҫ���ó�ʱ��ʱ��̫���������ĳ�ʱʱ����û��������������Cache�������Ǹ�����ص����ݵĶ�ȡ���ڵ���Ϣ�߶���صġ�
2. **ȥ������**����������Ļ�������ݲֿ�������������Ǻ����޵ģ����ԣ�������ԵĻ���Cache���Ƴ����ݡ�������Cache�����LRU�Ĳ������Ƴ������е����ݣ���Ȼ���Ƴ��Ĳ���Ҳ�ǿ����Զ���ġ�����ȫ�ֵĹ������Ժͻ�����������ԣ�����ȷ��Cache���ĵ��ڴ���Դ�Ǹ�Ц�ġ���Ȼ��ͨ������ֻ����һ��ȫ�ֵĹ��ڲ��ԣ���������˵����ͬ�Ļ�������ݲֿ��л�ȡ��Դ�ܰ����ʱ����������ʵĻ���
3. **׼��Cache**���ܶ�Ľ����������������Ӧ�������Ĺ����У��ͽ����ݲֿ��е�����д�뵽����֮�С�Cache-Asideģʽ������һЩ���ݹ��ڻ����Ƴ����������Ȼ�ܶ�ʱ���Ǻ����õġ�
4. **һ����**��ʵ��Cache-Asideģʽ�����ܱ�֤Cache�����ݲֿ�֮�������һ���ԡ���Ϊ���ݲֿ��е����ݿ������κε�ʱ�򶼿����������������޸ģ�������޸Ĳ��ἰʱ�ķ�ӳ��Cache�ϣ�ֻ������һ��Cache�����ݲֿ��и������ݵ�ʱ��Ż��н��������ݲ�һ�µ����⡣������ݲֿ�������Ƶ���ɷ�Cahce������µĻ�����������ͬ������hi��ø������ԡ�
5. **���أ��ڴ棩����**��CacheҲ�ǿ�������Ӧ�ñ�������ġ�Cache-Asideģʽ��һЩӦ��Ƶ��������ͬ�����ݵ�ʱ��������Ч��Ȼ��������Cache����Ӧ��˽�еģ�������ÿ��Ӧ���ж��еĶ���Ŀ���������������ݿ��ܺܿ��ڲ�ͬ��Ӧ���оͲ�һ���ˣ�����ˢ�µ�Ƶ����ø�������֤һ�¡�����Щ����¿���ʹ�ù���Ļ��棬�е�ʱ��Ҳ����ʹ�ñ���Cache������ʹ����һ�־���Ҫ����ʵ�ʵĳ������жϡ�

## ʲôʱ��ʹ��Cache-Asideģʽ

ʲôʱ��ʹ�ã�

* ��Cache���ṩԭ����Read-Through��Write-Through������ʱ��
* ��Դ�������ǲ���Ԥ���ʱ��Cache-Asideģʽ��Ӧ�ÿ��Ը����������������ݡ�����Ӧ������ʲô���ݣ�����Ҫ��ǰ�������衣

## �������

��Windows Azure�п����߿���ʹ��Windows Azure Cache������һ���ֲ�ʽ���ɶ��Ӧ��ʵ������Ļ��档��������е�`GetMyEntityAsync`������ʵ�־���һ������Windows Azure Cache��Cache-Asideģʽ��ʵ�֡��������ͨ��Read-Through�ķ�ʽ�ӻ����л�ȡ���ݡ�
һ������ͨ��һ������ID��ΪKey���޶���`GetMyEntityAsync`�����������key������һ���ַ���Key(Windows Azure Cache API��ͨ���ַ�������ΪKeyֵ��)��Ȼ����ͨ�������������ȡ��Ӧ�Ķ�������ҵ���ƥ����Ŀ����ô��ֱ�ӷ���ƥ����Ŀ�����������û��ƥ����Ŀ��`GetMyEntityAsync`����������ݲֿ��л�ȡ����Ȼ�����������ӵ�����֮�У�Ȼ���ٷ���������������������ݲֿ��л�ȡ����Ĳ�����ʡ���˵ģ���Ϊ����ģʽ�����ݲֿ�ķ����Ƕ����ģ�����Ҫע����ǣ��������Ŀ�����ð�����˳��ģ���ֹ���泤�ڲ�����Ҳ����ʹ�������Դ�˷ѡ�

```
private DataCache cache;
...
public async Task<MyEntity> GetMyEntityAsync(int id)
{
    // Define a unique key for this method and its parameters.
    var key = string.Format("StoreWithCache_GetAsync_{0}", id);
    var expiration = TimeSpan.FromMinutes(3);
    bool cacheException = false;
    try
    {
        // Try to get the entity from the cache.
        var cacheItem = cache.GetCacheItem(key);
        if (cacheItem != null)
        {
            return cacheItem.Value as MyEntity;
        }
    }
    catch (DataCacheException)
    {
        // If there is a cache related issue, raise an exception
        // and avoid using the cache for the rest of the call.
        cacheException = true;
    }
    // If there is a cache miss, get the entity from the original store and cache it.
    // Code has been omitted because it is data store dependent.
    var entity = ...;
    if (!cacheException)
    {
        try
        {
            // Avoid caching a null value.
            if (entity != null)
            {
                // Put the item in the cache with a custom expiration time that
                // depends on how critical it might be to have stale data.
                cache.Put(key, entity, timeout: expiration);
            }
        }
        catch (DataCacheException)
        {
            // If there is a cache related issue, ignore it
            // and just return the entity.
        }
    }
    return entity;
}
```

> ���������ʹ���� Windows Azure Cache��API���������ݲֿ�ʹ�Cache�л�ȡ���ݡ����˽�������Windows Azure Cache API����Ϣ�����Բο�MSDN�����[ʹ��Windows Azure ����](http://msdn.microsoft.com/library/windowsazure/hh914165.aspx)��

����չʾ��`UpdateEntityAsync`����չʾ�������Ӧ���޸Ļ��������ʧЧ�ġ���Ҳ��Write-Through������һ�����������еĴ����������ݲֿ⣬Ȼ��ͨ������`Remove`�������Ƴ����ݲֿ��е����ݡ�

> ע�⣺�������һЩ������˳����ʮ����Ҫ�ġ���������ȱ��Ƴ��ˣ�Ȼ�󻺴�Ÿ��µĻ����ͻ����ڻ�ȡ���ݵ�ʱ�����һ���ĸ���
ֱ�Ӵ����ݲֿ������ȡ��û�и��µĹ������ݡ�

```
//C# 
public async Task UpdateEntityAsync(MyEntity entity) 
{  
    // Update the object in the original data store  
    await this.store.UpdateEntityAsync(entity).ConfigureAwait(false);  
    
    // Get the correct key for the cached object.  
    var key = this.GetAsyncCacheKey(entity.Id);  
    
    // Then, invalidate the current cache object  
    this.cache.Remove(key); 
} 

private string GetAsyncCacheKey(int objectId) 
{  
    return string.Format("StoreWithCache_GetAsync_{0}", objectId); 
}

```