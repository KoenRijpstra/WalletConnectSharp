# WalletConnectSharp

WalletConnect sharp is an implementation of the [WalletConnect](#) protocol using .NET and NEthereum. 

## Usage

First you must define the `ClientMeta` you would like to send along with your connect request. This is what is shwon in the Wallet UI

```
var metadata = new ClientMeta()
{
    Description = "This is a test of the Nethereum.WalletConnect feature",
    Icons = new[] {"https://app.warriders.com/favicon.ico"},
    Name = "WalletConnect Test",
    URL = "https://app.warriders.com"
};    
```

Once you have the metadata, you can create the `WalletConnect` object

```
var walletConnect = new WalletConnect(metadata);
Console.WriteLine(walletConnect.URI);
```

This will print the `wc` connect code into the console. You can transform this text into a QR code or use it for deep linking. Once you have it displayed and setup, you can then call `connect()`

```
var walletConnectData = await walletConnect.Connect();
```

This function returns a `Task<WCSessionData>` object, so it can be awaited if your using async/await. The `WCSessionData` has data about the current session (accounts, chainId, etc..)

```
Console.WriteLine(walletConnectData.accounts[0]);
Console.WriteLine(walletConnectData.chainId);
```

## Connecting with NEthereum

With the above, you have enough to use the base WalletConnect protocol. However, this library comes with an NEthereum provider implementation. To use it, you simply invoke `CreateProvider(url)` or `CreateProvider(IClient)`. This is because the `WalletConnect` protocol does not perform read operations, so you must provide either an `Infura Project ID`, a node HTTP url for `HttpProvider` or a custom `IClient`.

Here is an example
```
var web3 = new Web3(walletConnect.CreateProvider(new Uri("https://mainnet.infura.io/v3/<infruaId>"));
```
