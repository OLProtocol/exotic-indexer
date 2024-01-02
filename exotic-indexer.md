# 总结
## 1、ORD的同类产品的GOLANG实现
## 2、数据采集来自全节点bitcoind,并存储于badger(类leveldb)数据
## 3、部分接口要额外用esplora区块浏览器(https://blockstream.info/testnet)查询外网或者自建区块浏览器index服务

# 接口分析
## 1 https://docs.bitgem.tech/endpoints/scan-address 返回指定地址的所有交易的BTC基础信息
参考 
https://testnet.ordinals.com/output/96d72a8a4198177859c42b062f3e01f4c9c12720fc0d80776f7e4717054e5fb1:0
https://blockstream.info/testnet/address/tb1qcvts8jpr437382gp74vrsaa9du6tvceaz74p7j
```shell
curl -X POST -H "Content-Type: application/json" -d '{"address": "tb1qcvts8jpr437382gp74vrsaa9du6tvceaz74p7j","excludeCommonRanges": false}' http://192.168.1.111:8080/address-ranges
{
    "ranges": [
        {
            "utxo": "2e4ef5cf2977066d76f1a5593e42a2e734728f3f2d1766142964ade43cd4c3ee:0",
            "start": 1216839877604976,
            "size": 330,
            "end": 1216839877605306,
            "offset": 0
        },
        ...
    ],
    "exoticRanges": []
}
```

## 2 https://docs.bitgem.tech/endpoints/scan-utxos
返回指定utxo集合的BTC基础信息
```shell
curl -X POST -H "Content-Type: application/json" -d '{"utxos": ["2e4ef5cf2977066d76f1a5593e42a2e734728f3f2d1766142964ade43cd4c3ee:0"], "excludeCommonRanges": false
}' http://localhost:8080/utxo-ranges

curl -X POST -H "Content-Type: application/json" -d '{"utxos": ["2e4ef5cf2977066d76f1a5593e42a2e734728f3f2d1766142964ade43cd4c3ee:0", "41145212e583d394cf2ecb06abb236e63149030e2f3f709a6c2ae85e4695e60e:0"], "excludeCommonRanges": false
}' http://localhost:8080/utxo-ranges

{
    "ranges": [
        {
            "utxo": "2e4ef5cf2977066d76f1a5593e42a2e734728f3f2d1766142964ade43cd4c3ee:0",
            "start": 1216839877604976,
            "size": 330,
            "end": 1216839877605306,
            "offset": 0
        },
        {
            "utxo": "41145212e583d394cf2ecb06abb236e63149030e2f3f709a6c2ae85e4695e60e:0",
            "start": 1216839877606056,
            "size": 330,
            "end": 1216839877606386,
            "offset": 0
        }
    ],
    "exoticRanges": []
}
```

## 3 https://docs.bitgem.tech/endpoints/sat-properties
返回指定聪的属性（似乎不够，比如没有rarity、inscriptions、location）参考https://testnet.ordinals.com/sat/1216839877606056
curl http://192.168.1.111:8080/sat/1216839877606056
{"sat":1216839877606056,"height":276735,"cycle":0,"epoch":1,"period":137,"satributes":[]}

curl https://api.bitgem.tech/sat/1216839877606056
{"sat":1216839877606056,"height":276735,"cycle":0,"epoch":1,"period":137,"satributes":[]}



## 4 https://docs.bitgem.tech/endpoints/supported-satributes
返回支持的聪属性
curl https://api.bitgem.tech/info/satributes
["pizza","block9","block78","nakamoto","first_transaction","vintage","common","uncommon","rare","epic","legendary","mythic","black","alpha","omega","hitman","jpeg","fibonacci"]%  

# office
https://www.bitgem.tech/
https://docs.bitgem.tech/
https://api.bitgem.tech/
https://github.com/BitGemTech/exotic-indexer
