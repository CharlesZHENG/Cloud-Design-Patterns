# Circuit Breaker

Circuit Breakerģʽ�ᴦ��һЩ��Ҫһ��ʱ��������Զ�̷����Զ����Դ�Ĵ��󡣸�ģʽ�������һ��Ӧ�õ��ȶ��Ժ͵��ԡ�

## ����

���������Ƶķֲ�ʽ�����У���һ��Ӧ����Ҫִ��һЩ����Զ����Դ������Զ�˷����ʱ���Ǻ���������һЩżȻ�Ĵ���ģ�����˵�����������ٶȺ�������ʱ����������Դ�Ĺ���ʹ�ã�������ʱ��Դ���ڿ��õȵȡ���һ��Ĵ���ͨ����˵���ڶ��ݵ�ʱ���ڣ��Զ��ظ�������һ����׳����Ӧ��Ҳ���ܹ�ͨ��һЩ�����ܹ�����������󣬱���ʹ��**����**ģʽ��

Ȼ����Ҳ��һЩ����������ǳ���һЩ���벻�����¼��������¼�����Ԥ�ڣ�������Ҫ���ĺܶ��¼�����������Щ��������������Ҳ���ԴӶ�ʧ�������ӵ����������ʧ�ܡ�����Щ����£���Ӧ�ü������Ի���ִ�в������Ѿ�û�������ˡ���Եģ�Ӧ��Ӧ��Ѹ�ٽ��շ����ʧ�ܣ������Ը��ݴ�����������ȡ��Ӧ�Ĵ�ʩ��

���⣬���һ������ǳ���æ��ϵͳ�Ĳ��ִ�����ܻᵼ��ѩ��ЧӦ��������˵��һ��������������Ĳ����������ó�ʱʱ��ģ����������һ��ʱ���޷�Ӧ�𣬵��÷����Է��ش�����Ϣ�ġ�Ȼ��������Կ��ܵ��´����������������������ֱ��Timeoutʱ�䵽�ˡ���Щ������������ܻ����ϵͳ�ؼ�����Դ�������ڴ棬�̣߳����ݿ����ӵȵ���Ϣ����ˣ����õ���ԴҲ���ܻᱻ�ľ������ϵͳ����������ز��ֵ�ʧ�ܡ�����Щ����£���õķ���������Щ���ó�ʱ�Ĳ�������ʧ�ܣ�����ֻ�е�������ܳɹ���ʱ���ȥ���á���Ȼ�����ý϶̵ĳ�ʱʱ��Ҳ�ܸ�����һ���⣬���ǳ�ʱʱ�䲻������̫�̣���������ĵ��÷�������Ϊ�����ĳ�ʱ��ʧ�ܡ�

## �������

Circuit-Breakerģʽ���Է�ֹӦ���ظ��ĳ��Ե�������ʧ�ܵĲ�������Circuit-Breakerģʽ�жϴ��������ʱ��������������ٳ����ȴ�ȥ�˷�CPU��Դ����Ȼ��Circuit-BreakerģʽҲ��Ӧ�ñ�����Է��ִ�����û�б��޸�����������������Ѿ����޸��ˣ�Ӧ�ÿ������³���ȥ���÷���

> Circuit-Breakerģʽ��Ŀ�ĺ�Retryģʽ��Ŀ���ǲ�ͬ�ġ�Retryģʽ��Ӧ�ò��ϵ����Ե��ã�ֱ�����ɹ�����Circuit-Breakerģʽ����ֹӦ�ü������������������Ӧ�ÿ���ͬʱʹ������ģʽ��Ȼ���������߼�Ӧ�ö������е�Circuit-Breaker���ص��쳣ʮ�����У�����������Circuit-Breaker���ִ����ʱ���޷��޸��������ֱ�Ӳ��ټ������ԡ�

Circuit-Breaker�����þͺ��ƿ���ʧ�ܲ����Ĵ������������������Ĵ���Ȼ��������һ��Ϣ�������Ƿ���������ļ���ִ�У�����ֱ�����̷����쳣��Ϣ��

Circuit-Breaker���԰������µ�״̬��ģ��һ����·����ʵ�֣�

* **�ر�**��Ӧ�õ������Ѿ�·�ɵ����������������Ӧ��ά�����һ��ʱ��Ĵ�����Ϣ��������ò���ʧ�ܣ���ô�����������������Ϣ������������������������������ʱ�����ֵ��������뵽**��**״̬�����ʱ�򣬴�������һ����ʱ��Timer����Timer�����ˣ����������**�뿪**״̬��

> ��ʱTimer��Ŀ����Ϊ�˸�ϵͳһ��ʱ���������޸�֮ǰ���������⡣

* **��**������ÿ���ʧ�ܵĲ�������ʧ�ܣ����еĵ���ֱ�����쳣��Ӧ�á�
* **�뿪**��ֻ��һ��������Ӧ��������Խ��в����ĵ��á������Щ����ɹ��ˣ���ô�ͼٶ�֮ǰ���ɵĴ����Ѿ���ϵͳ�Զ��޸��ˣ���Circuit-Breakerת����**�ر�**״̬��ͬʱ���ô����������������κ�����ʧ���ˣ���ôCircuit-Breaker��ٶ�������Ȼ�ڴ��ڣ�Circuit-Breaker������ת����**��**״̬����������ʱTimer��ϵͳ�����ʱ���������޸�����
> **�뿪**״̬���Ժ���Ч����ֹһ�����Իָ��ķ��񱻴�������������û�������ڻָ��еķ��񣬿���Ҳ�ܹ�����һ������������ֱ����ȫ�ָ����ָܻ�ȫ���������������ǣ�ͻȻ�����Ĵ���Ҳ���ܻ���ָ��еķ�������crash����

�ο�����״̬�任ͼ��

![](Circuit-Breaker.png)

��Ҫע����ǣ���ͼ�У��ر�״̬���õĴ���������ǻ���ʱ��ġ�������һ����ʱ���������á���Ҳ�ܹ��ڳ������������²���Circuit-Breakerģʽ�����״̬�������������ֵ�Ż���Circuit-Breaker���뵽��״̬��ֻ�е�ָ��ʱ�����ڣ���������ﵽ��ֵ������Circuit-Breaker���뵽��״̬���뿪״̬��ʹ�õĳɹ�����������¼�ɹ��ĵ��ô�����Circuit-Breaker�����֮������������ĳɹ��ĵ��ã���ôCircuit-Breaker�ͻ����ر�״̬������κε��õ�ʧ���ˣ���ôCircuit-BreakerҲ�����½��뵽��״̬���ɹ�������Ҳ�����ã�ֱ���´����½��뵽�뿪״̬��

> ͨ��ϵͳ���ⲿ�ָ����ܶ�ʱ����ͨ������ʧ�ܵ���������޸�������������ɵġ�

ʵ��Circuit-Brakerģʽ��������ϵͳ���ȶ��Ժ͵��ԣ���ϵͳ�Ӵ���ָ���ʱ�򣬿��Ծ���������ʧ�ܶ�ϵͳ���ܵ�Ӱ�졣Circuit-Breakerģʽ����ͨ���ܾ��ⲿ��������֤�������Ӧʱ�䣬�����ǵȴ������ĳ�ʱ�����߳��������������Circuit-Breaker��ÿһ��״̬�ı��ʱ������һЩ�¼��Ļ������״̬�ĸı�Ҳ������������Circuit-Breaker����ģ��Ľ���״̬�������ǶԼ��Circuit-Breaker�Ĺ���Ա�������棬Circuit-Breaker�Ѿ������˴�״̬��

Circuit-Breakerģʽ���ԺܺõĶ��Ʋ�����ܶ���ܵĴ��󡣾�����˵�������߿���Ӧ��һ�������ĳ�ʱTimer��Ҳ����ֱ����Circuit-Breaker�ڴ��ڴ�״̬���룬���������֮��û�н�����ͳ�ʱ�����ӵȵȡ�����Щ�����£���״̬��Circuit-BreakerҲ���Բ��׳��쳣���Ƿ���Ĭ��ֵ������Ӧ�õ���Ӧ��

## ��Ҫ���ǵ�����

��������ʵ��Circuit-Breakerģʽ��ʱ�������µ�һЩ�ط���Ҫע�⣺

* **�쳣�Ĵ���**��Ӧ�����ͨ��Circuit-Breaker�����ò����Ļ����ͱ����ܹ��������ʧЧ��������쳣����������Щ�쳣�Ĵ��뽫���Ӧ���Ǹ߶���صġ�������˵��Ӧ�ÿ���ѡ����ʱ��������񣬵��������Ĳ������ﵽ�����ͬ�����񣬻��߻�ȡ��ͬ�����ݣ������׳��쳣���û������Ժ����ԡ�
* **�쳣������**������ʧ�ܿ������ж��ԭ����Щ������ܻ�������Ĵ�������ء�������˵������ʧ�ܿ�������ΪԶ�˵ķ���creash���ˣ���Ҫ������ʱ�����ָ�������ֻ����Ϊ������ض���ɵ���ʱ�Է���ʱ��Circuit-BreakerҲ�ܹ��жϴ�������ͣ���������ͬ�Ĳ��ԡ�������˵����Գ�ʱ���õĴ��������ֵ�������õıȷ���ʧЧ����ֵ���ߡ�
* **��־**��Circuit-BreakerӦ�ð����е�ʧ�����󶼼�¼��־���Ϳ��ܳɹ������󣩣����������ù���Ա��ص��ⲿ���õĽ���״̬��
* **�ָ���**��������Ӧ��Ϊ�䱣����Զ�˵��ý��к����������ƥ��Զ�˵��õĻָ�ģʽ��������˵�����Circuit-Breaker���õ�ͣ���ڴ�״̬�ܾõĻ�������Զ�˷����Ѿ������ˣ���ΪCircuit-Breaker�Ĵ�״̬����������״̬��Ȼ���ڲ�����״̬�����Ƶģ��������Circuit-Breaker�Ļָ�ʱ��̫�죬Ҳ����Ӧ���ڴ�״̬�Ͱ뿪״̬֮�䲻���𵴡�
* **����ʧЧ����**���ڴ�״̬�������ʹ��Timer���ж�ʲôʱ��ת��Ϊ�뿪״̬��Circuit-BreakerҲ����ѡ�����Ե�pingԶ�˵ķ�����Դ�����������Ƿ���á�ping����Ҳ������Ϊһ�ֳ���Զ�˵��õķ�ʽ����Ȼ��Ҳ������Զ�˷����ṩ�������ӿ������Է����Ƿ�����������Health-Endpoint���ģʽ������������
* **�ֶ�����**����ĳ��ϵͳ�У������ʧЧ��ʱ���ǿɼ��ģ�Circuit-BreakerҲ�����ṩһЩ�ֶ��ָ���ѡ��������Աǿ�ƵĹر�Circuit-Breaker�����Ƶģ�����ԱҲ������Զ�˷�����ʱʧЧ�������ǿ��������Circuit-Breaker�����״̬��������ʱTimer��
* **����**��ͬһ��Circuit-Breaker�ܿ��ܱ�Ӧ�õ�ʵ�������������ʡ�������ʵ��Ӧ���Ƿ������Ļ��߶���ÿ���������Ӹ�������ѡ�
* **��Դ�Ĳ�ͬ**����Ҫע����ǣ���Ϊһ�����͵���Դ����һ��Circuit-Breaker���ǿ����ж���������ṩ��Դ�ķ���ʱҪ����С�ġ�������˵����һ���������Shard�����ݲֿ��У����е�һ��Shardû�������һ�����ܶ�ʱ���ڷ��������⡣�������ķ���������ĳ����л������һ�𣬼�ʹ��������ƣ�Ӧ��Ҳ�᳢�Է��ʣ��Ӷ������ġ�
* **���ٶ�·**���е�ʱ�򣬴����Ӧ����Ϣ�㹻�жϵ�ǰ��״̬����Circuit-Breaker���̴������ٸ����ӣ����һ��Shard�ķ�����Ϣ��ʾ���������������ԣ�ϣ���ڼ����Ӻ����Ե�ʱ����ôCircuit-Breaker�Ͳ���Ҫ������������ֵ�ڽ���Open״̬�ˡ�
> HTTPЭ�鶨�壺503��ʾ���񲻿��ã��������ķ���ǰ��web���������治���ã��Ϳ��Է���503�����Ӧ����Ϣ�Ϳ��԰����������Ϣ�������������ӳ�����ʱ�䡣

* **����ʧ������**����**��**״̬��������÷�����ٵ�ʧ�ܣ�Circuit-BreakerҲ���Լ�¼���������Ȼ�����Ժ��ʱ�䣬��������Щʧ�ܵ�������������
* **�ⲿ�����ϵĲ�ǡ����ʱʱ��**����������ⲿ����ĳ�ʱʱ�����õĹ����Ļ���Circuit-Breaker���ܺ��ѱ���Ӧ�á������ʱʱ�����������Circuit-Breaker���߳̿�������Ϊ����ʧ��֮ǰ���ͱ���ȫ�����ˡ���������£�Ӧ�õ�ʵ��������Circuit-Breaker�����ˣ�������Open״̬��Ҳ�����൱�������̴߳�������״̬�ġ�

## ��ʱʹ�ø�ģʽ

ʹ�ø�ģʽ��

* ����Ҫ��ֹӦ�ò��ϳ��Ե���Զ�˷�����߷��ʹ�����Դ��������Щ���������ʧ�ܵ�ʱ��ʹ��Circuit-Breakerģʽ�ܺ��ʡ�

ʲô�������ʺ�ʹ�ø�ģʽ��

* ������������ʱ�����Դ�������ڴ��е����ݽṹ��ʱ�򣬲��ʺ�ʹ�á������ֳ����£�Circuit-Breakerֻ���Ӧ�ô�������ĸ�����
* ��Circuit-Breaker��Ϊ����Ӧ���е�ҵ���߼��е��쳣�����һ����Ҳ�ǲ����ʵġ�

## Circuit-Breakerʹ�þ���

��webӦ���У���Щҳ������Ҫ���ⲿ�ķ�������ȡ���ݵġ����ϵͳʵ������С��ȵĻ��棬��ôҳ��Ĵ������ʿ��ܾͻ���������ĵ��á����webӦ�ú��ⲿ����֮�������˳�ʱ������60s���Ļ�������ⲿ����û����Ӧ��ҳ�����Ϊ����ʧЧ���׳��쳣��
Ȼ�����������ʧ���ˣ���ϵͳ��Ȼ�ǳ���Ƶ�����ʣ��û����ܻᱻ�ȵȴ�60�룬Ȼ�󿴵��޽������������ڴ棬���������Լ��̵߳���Դ�������ˣ������û����ٷ����ⲿ��Դ�����ܷ���Ҳ�ᱻ�ܾ��ġ�
��Ȼ������web��������ʹ�ø��ؾ���ȷ�ʽ����һ���̶��Ϸ�ֹ��Դ�ĺľ������ǣ�������Ȼ�޷�����û���ʱ��ȴ�û����Ӧҳ������⡣
ͨ��ʹ��Circuit-Breaker�����������ⲿ������߼�������Ч���������ᵽ�����⡣�û����󽫻�ʧ�ܣ��������������ʧ�ܣ����ǲ��ᵼ��������Դ��������
`CircuitBreaker`��ͨ���ڲ�һ��`ICircuitBreakerStateStore`������ά��Circuit-Breaker��״̬��Ϣ���ο����´��룺

```
interface ICircuitBreakerStateStore
{
    CircuitBreakerStateEnum State { get; }
    Exception LastException { get; }
    DateTime LastStateChangedDateUtc { get; }
    void Trip(Exception ex);
    void Reset();
    void HalfOpen();
    bool IsClosed { get; }
}
```
���е�`State`���Ա�ʾCircuit-Breaker��ǰ��״̬�����а���ǰ�����ᵽ������״̬`Open`,`HalfOpen`,`Closed`��`IsClose`������״̬Ϊ`Closed`��ʱ��ͻ᷵��`true`��`Trip(Exception ex)`�����ὫCircuit-Breaker��״̬��ת����`Open`��״̬�����Ҽ�¼����״̬�仯���쳣��Ϣ���Լ������쳣��ʱ�����Ϣ��`LastException`�����Լ�`LastStateChangeDateUtc`���Ծ���������ȡ״̬ת�����쳣�Լ�ʱ����Ϣ�ġ�`Reset()`�������ر�Circuit-Breaker��`HalfOpen()`�������ǽ�Circuit-Breaker��״̬��Ϊ`HalfOpen`��

```
public class CircuitBreaker
{
    private readonly ICircuitBreakerStateStore stateStore =
        CircuitBreakerStateStoreFactory.GetCircuitBreakerStateStore();
    private readonly object halfOpenSyncObject = new object ();
    ...

    public bool IsClosed { get { return stateStore.IsClosed; } }
    
    public bool IsOpen { get { return !IsClosed; } }
    
    public void ExecuteAction(Action action)
    {
        ...
        if (IsOpen)
        {
            // The circuit breaker is Open.
            ... (see code sample below for details)
        }
        // The circuit breaker is Closed, execute the action.
        try
        {
            action();
        }
        catch (Exception ex)
        {
            // If an exception still occurs here, simply
            // re-trip the breaker immediately.
            this.TrackException(ex);
            // Throw the exception so that the caller can tell
            // the type of exception that was thrown.
            throw;
        }
    }
    private void TrackException(Exception ex)
    {
        // For simplicity in this example, open the circuit breaker on the first exception.
        // In reality this would be more complex. A certain type of exception, such as one
        // that indicates a service is offline, might trip the circuit breaker immediately.
        // Alternatively it may count exceptions locally or across multiple instances and
        // use this value over time, or the exception/success ratio based on the exception
        // types, to open the circuit breaker.
        this.stateStore.Trip(ex);
    }
}
```

`CircuitBreaker`�ᴴ��һ��ʵ��`ICircuitBreakerStateStore`��ʵ����ά��CircuitBreaker��״̬�����е�`ExecuteAction(Action action)`���������һ�����ܳ���ķ�����������������е�ʱ�򣬻����ȼ��Circuit-Breaker��״̬������ǹر�״̬�����������ִ��Զ�˷���ĵ��á�����������ʧ�ܵ��ˣ����ͨ��`TrackException(Exception ex)`������`Circuit-Breaker`��״̬��Ϊ��״̬������ο�IsOpen�еĴ��룺

```
if (IsOpen)
{
    // The circuit breaker is Open. Check if the Open timeout has expired.
    // If it has, set the state to HalfOpen. Another approach may be to simply
    // check for the HalfOpen state that had be set by some other operation.
    if (stateStore.LastStateChangedDateUtc + OpenToHalfOpenWaitTime < DateTime.UtcNow)
    {
        // The Open timeout has expired. Allow one operation to execute. Note that, in
        // this example, the circuit breaker is simply set to HalfOpen after being
        // in the Open state for some period of time. An alternative would be to set
        // this using some other approach such as a timer, test method, manually, and
        // so on, and simply check the state here to determine how to handle execution
        // of the action.
        // Limit the number of threads to be executed when the breaker is HalfOpen.
        // An alternative would be to use a more complex approach to determine which
        // threads or how many are allowed to execute, or to execute a simple test
        // method instead.
        bool lockTaken = false;
        try
        {
            Monitor.TryEnter(halfOpenSyncObject, ref lockTaken)
            if (lockTaken)
            {
                // Set the circuit breaker state to HalfOpen.
                stateStore.HalfOpen();
                // Attempt the operation.
                action();
                // If this action succeeds, reset the state and allow other operations.
                // In reality, instead of immediately returning to the Open state, a counter
                // here would record the number of successful operations and return the
                // circuit breaker to the Open state only after a specified number succeed.
                this.stateStore.Reset();
                return;
            }
        }
        catch (Exception ex)
        {
            // If there is still an exception, trip the breaker again immediately.
            this.stateStore.Trip(ex);
            // Throw the exception so that the caller knows which exception occurred.
            throw;
        } 
        finally
        {
            if (lockTaken)
            {
                Monitor.Exit(halfOpenSyncObject);
            }
        }
    }
}
```

�����Circuit-Breaker�Ĳ��Ժܼ򵥣����ǵȴ�һ����ʱ�䣬Ȼ��Ž���`HalfOpen`״̬�����`action()`�ɹ��ˣ������»ָ���`Close`״̬�����`action()`ʧ������Circuit-Breaker���½��뵽`Open`״̬��

## ��ص�����ģʽ
�����ģʽ��Circuit-BreakerģʽҲ����صģ�

* **Retryģʽ**������ģʽ����Circuit-Breakerģʽ��һ����������Ҫ����������ǵ�����Զ�̷��񲻿��õ�ʱ����Ӧ��������������Ԥ�ڵĶ�ʱ��Ĵ���
* **Health Endpoint Monitoringģʽ**��Circuit-Breaker����ͨ����������Զ�˵ķ����ṩ������ķ�������ض������ĵĽ���״̬���÷�����Ҫ����һЩ��Ϣ��չʾ�佡����״̬��
