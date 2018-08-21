# 智能合约参数和返回值

在智能合约发布或调用时，需要指定智能合约的参数，智能合约参数是 byte 类型，定义如下。

```c#
    /// <summary>
    /// 表示智能合约的参数类型
    /// </summary>
    public enum ContractParameterType : byte
    {
        /// <summary>
        /// 签名
        /// </summary>
        Signature = 0,
        Boolean = 1,
        /// <summary>
        /// 整数
        /// </summary>
        Integer = 2,
        /// <summary>
        /// 160位散列值
        /// </summary>
        Hash160 = 3,
        /// <summary>
        /// 256 位散列值
        /// </summary>
        Hash256 = 4,
        /// <summary>
        /// 字节数组
        /// </summary>
        ByteArray = 5,
        PublicKey = 6,
        String = 7,
        
         /// <summary>
         /// 对象数组
         /// </summary>        
        Array = 10,
        InteropInterface = 0xf0,
        Void = 0xff
    }
```

如智能合约定义为：

```c#
public class Lock : SmartContract
{
    public static bool Main(int a, bool b, byte[] pubkey, byte[] signature)
    {
        // 略……
    }
}
```

通过上表可查到，int 为 2，bool 为 1，公钥字节数组为 6，签名字节数组为 0。

在 NEO-GUI 客户端发布智能合约填写参数时，每个参数用两位 16 进制字符表示，所以上面的智能合约的形参列表表示为：02010600，返回值为：01。


另一种很常用的智能合约形式：

```c#
public class Contract1 : SmartContract
{
    public static object Main(string operation, object[] args)
    {
        // operation 是操作名字，以代币系统为例，有"deploy"，"balanceOf"，"transfer"等
        // args 是操作参数，[参数1，参数2....]
    }
}
```

智能合约的形参列表为 String（字符串） 和 object[] （对象数组） 对应表示为：0710
返回值为 object （对象） 可以使用字节数组的形式读取，表示为 ：05

> 在实际返回中，可能根据选取操作（operation）的不同而需要返回不同类型的返回值。此处定义Main时，可以笼统地用Object来表示返回值的类型。
> 在返回参数的时候使用字节数组形式，是因为所有类型都可以当做字节数组顺利输出。但是在读取时，要注意你看到的是字节数组，你需要将字节数组重新变化为对应类型。（注意部分类型会逆序存储，010203 -> 030201）
