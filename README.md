# UcAsp.Opc
opc for da and ua
## 注 意：
[OPC.DA 环境下先安装lib文件夹下的：Core Components Redistributable (x64).msi ]

## Nuget 安装
```ps
Install-Package UcAsp.Opc -Version 1.0.0.5 
```
## OPC.DA 读取变量
```C#
OpcClient client = new OpcClient(new Uri("opcda://127.0.0.1/Matrikon.OPC.Simulation.1"));
string r = client.Read<string>("Random.String");

```

## OPC.UA 读取变量

```C#
OpcClient client = new OpcClient(new Uri("opc.tcp://127.0.0.1:26543/Workstation.RobotServer"));
float r = client.Read<float>("Robot1.Axis1");
var list = client.Read(new string[] { "Robot1.Axis1", "Robot1.Axis2" });
```

## 读取和写入

```C#
OpcClient client = new OpcClient(new Uri("opc.tcp://127.0.0.1:26543/Workstation.RobotServer"));
client.Write<float>("Robot1.Axis1", 2.0090f);
float r = client.Read<float>("Robot1.Axis1");
var result = client.Write(new string[] { "Robot1.Axis1", "Robot1.Axis2" }, new object[] { 2.0090f, 2.0090f });
```
##  分组监听数据变化
```C#
public void UAGroup()
{
    OpcClient client = new OpcClient(new Uri("opc.tcp://127.0.0.1:26543/Workstation.RobotServer"));
    OpcGroup group = client.AddGroup("Test");
    client.AddItems("Test", new string[] { "Robot1.Axis1", "Robot1.Axis2" });
    group.DataChange += Group_DataChange;
}


private void Group_DataChange(object sender, System.Collections.Generic.List<OpcItemValue> e)
{
  foreach (OpcItemValue o in e)
   {
       Console.WriteLine(o.Value);
   }
}

```
##  获取节点信息

```C#
OpcClient client = new OpcClient(new Uri("opc.tcp://127.0.0.1:26543/Workstation.RobotServer"));
INode root = client.RootNode;           
IEnumerable<INode> list = client.ExploreFolder(root.Tag);
IEnumerable<INode> server = client.ExploreFolder(list.ToList()[0].Tag);
IEnumerable<INode> s = client.ExploreFolder(server.ToList()[0].Tag);
```
