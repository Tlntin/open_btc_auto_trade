## 框架示意图

![GT4.1框架2](https://vbahome.cn/img/GT4.1框架2.png)

## 文件夹说明

```shell script
├─data  # 数据文件夹
│  ├─sql  # sql相关文件，包括创建和插入和删除表格的json
│  └─count.json  # 保存所有交易对买入卖出次数
│
├─flowrisk  # 计算vpin系数的模块
│
├─tools  # 储存较多辅助交易函数的文件夹
│  ├─ccxt_api.py  # 基于ccxt_api二次构建的一个交易函数方法，目前仅支持huobi和gate.io,可以自行扩展
│  ├─main.py  # 邮箱发送相关方法
│  ├─predict.py  # 价格监测与趋势预测的模块
│  ├─sql_create.py  # sql语法创建的文件
│  ├─trade.py  # 正式交易的相关文件
│  └─user_input.py  # 用户自定义相关的相关信息
│
├─data_manager.py  # mysql数据管理以及邮箱监测
│
└─main.py  # 主函数入口，需要手动修改里面的交易所
```




## 使用说明

- 首次使用需要配置tools文件夹下的“user_input.py”文件，填写好里面的api_key，secret_key信息。其中query_url与trade_url可以不修改，或者看情况而定。如果使用gate.io交易所，则无需修改，如果使用其它交易所，则需要修改tools文件夹下的ccxt_api.py文件，注释掉29行与30行。

- user_input.py文件下的trade_symbol交易对列表，根据自己的需求填写，但注意不要填错了，并且注意使用英文单引号

- data_manager.py文件为数据管理文件，需要填写你的数据库信息，在18行至23行填写即可，目前默认为mysql，后面可能会开发其它便捷的数据库比如sqlite。注意修改第99行的send_email方法，这个是你的邮箱相关信息，注意仔细填写，建议使用163邮箱。

- 修改正确后，请运行tools文件夹下的sql_create.py文件生成相关sql数据库数据。然后将data_manager.py原有主函数注释，将下面的建表代码取消注释，如下面所示：

  ```python
  # 原来的代码
  if __name__ == '__main__':
      while True:
          data_manager = DataManger()
          data_manager.check_status()
          print('等待8分钟后检查')
          time.sleep(8 * 60)  # 每隔8分钟检测1次
          print('')
  
      # data_manager = DataManger()
      # for i in ['trade_history', 'coin_info', 'profit_info']:
      #     data_manager.creat_table(i)
  ```

  ```python
  # 改完后的代码
  if __name__ == '__main__':
      # while True:
      #     data_manager = DataManger()
      #     data_manager.check_status()
      #     print('等待8分钟后检查')
      #     time.sleep(8 * 60)  # 每隔8分钟检测1次
      #     print('')
  
      data_manager = DataManger()
      for i in ['trade_history', 'coin_info', 'profit_info']:
          data_manager.creat_table(i)
  ```

- 之后运行data_manager.py文件开始对建立相关数据库表格。正常来说应该不会报错。建表成功后，改回之前的注释情况，把建表的代码注释掉。

- 之后修改main.py里面的交易所名称，然后运行main.py文件即可开始，但是注意需要对api设置ip白名单，不然无法运行，会报错。

## 开源计划

- [ ] 前端开源
- [ ] 后端开源（mysql版）
- [ ] 后端增加sqlite版
- [ ] 后端增加windows 带GUI版

## 常见问题

- 待续。。。
