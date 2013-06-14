---
layout: post
title: Zookeeper事件监听的应用4
---
Zookeeper是一个分布式协作框架，它的应用场景很多，比如我们通常会用它来做一些集中式的配置管理，或者使用它作为一个全局性的序列化生成器，这些场景都比较简单。试想有一个场景如下：我们的应用在初始化的时候读取数据库数据并加载到到应用内存中以便程序快速响应。应用部署在多台服务器上，每台服务器部署多个应用实例，如何做到修改数据库的时候，同时通知所有应用实例去连接数据库并重新加载数据到内存呢？解决问题的思路有很多，但讲的是Zookeeper相关的，那么我们考虑使用Zookeeper的特性来完成。  
众所周知，Zookeeper基于选举算法实现。服务端采取主动推送方式，客户端使用长连接。每次推送都有事件，每个事件都有监听机制。  
模式简单表述为，触发事件，监听器监听，监听器修改。监听器实现了Zookeeper客户端和spring的两个listener接口，如下所示：  

    public class RouterDataListener implements ZkDataListener,ApplicationListener{
        ......
    }
	
以下为相关代码。  
###1、触发事件  
    public String updateData() throws Exception {
        String pin=LoginContext.getLoginContext().getPin();
        if(pin!=null){
            pin=pin.toLowerCase();
        }
        if(!zkWatcherDataService.addWatcherData(routerWatcherData,pin)){
            throw new Exception();
        }
        return null;
    }
	
###2、处理事件  
    public boolean addWatcherData(RouterWatcherData routerWatcherData,String pin) throws Exception{
        try {
            routerWatcherData=readerDataFromZk();
            if(routerWatcherData==null){
                routerWatcherData=new RouterWatcherData();
            }
            routerWatcherData.setCount(routerWatcherData.getCount());
            typeConfigService.saveConfig(configPath,routerWatcherData);
        } catch (Exception e) {
            logger.error("",e);
            return false;
        }
        return true;
    }
	
###3、注册监听器  
    public void onApplicationEvent(ApplicationEvent event) {
        String zkRouterPath=configPath+"/"+environment;
        try {
            jdZookeeperClient.subscribeDataChanges(zkRouterPath,this);
        } catch (Exception e) {
            logger.error("",e);
        }
    }
	
###4、处理监听事件  
    public void handleDataChange(String path, Object data) {
        businessBaseService.initialize();
        logger.info("数据初始化成功");
    }

总结：
Zookeeper的选举实现能使它应用于多种场景，比如序列号生成器，统一协作管理等。



