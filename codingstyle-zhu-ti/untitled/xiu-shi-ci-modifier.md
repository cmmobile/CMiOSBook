# 修飾詞\(Modifier\)

{% hint style="success" %}
任何類別和屬性都會有修飾詞，修飾詞也有順序

1. 存取修飾詞 \(public, private ....\)
2. override \(繼承才會到\)
3. dynamic \(串接OC或是KVO才會用到\)
4. mutating
5. lazy
6. final
7. static
8. owned \(weak, ....\)
{% endhint %}

```swift
public class GetMemberDataRequest: AuthRequest {
    
    public override var address: String {
        return "MemberData"
    }
}
```

{% hint style="success" %}
存取修飾詞只有internal不寫，其他都要寫 \(例如 private, public...\)
{% endhint %}

{% hint style="success" %}
Delegate一定要接weak \(原因請看ARC + Retain Cycle那篇\)
{% endhint %}

```swift
protocol DataManagerDelegate{
}

class DataManager{
    weak var delegate: DataManagerDelegate?
}
```



