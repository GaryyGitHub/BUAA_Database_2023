classDiagram
direction BT
class 仓库 {
   varchar(200) 地址
   varchar(100) 负责人
   int 仓库编号
}
class 供应商 {
   varchar(100) 名称
   varchar(100) 账号
   varchar(200) 地址
   int 供应商编号
}
class 商品 {
   varchar(100) 名称
   varchar(100) 类别
   varchar(100) 单位
   int 单价
   int 商品编号
}
class 存储 {
   int 库存
   date 日期
   int 安全库存量
   int 商品编号
   int 仓库编号
}
class 管理员 {
   varchar(50) 姓名
   text 业绩
   int 仓库
   int 编号
}
class 营业员 {
   varchar(50) 姓名
   text 业绩
   int 门店
   int 编号
}
class 进货 {
   varchar(100) 进货单号
   int 数量
   date 日期
   int 供应商
   int 商品
   int 仓库
}
class 配送 {
   varchar(100) 配送单号
   int 数量
   date 日期
   int 门店
   int 商品
   int 仓库
}
class 采购 {
   varchar(100) 采购单号
   int 数量
   date 日期
   int 采购员
   int 供应商
   int 商品
}
class 采购员 {
   varchar(50) 姓名
   text 业绩
   int 编号
}
class 销售 {
   varchar(100) 销售单号
   int 数量
   date 日期
   int 门店
   int 商品
   int 营业员
}
class 门店 {
   varchar(100) 名称
   varchar(200) 地址
   int 门店编号
}

存储  -->  仓库 : 仓库编号
存储  -->  商品 : 商品编号
管理员  -->  仓库 : 仓库:仓库编号
营业员  -->  门店 : 门店:门店编号
进货  -->  仓库 : 仓库:仓库编号
进货  -->  供应商 : 供应商:供应商编号
进货  -->  商品 : 商品:商品编号
配送  -->  仓库 : 仓库:仓库编号
配送  -->  商品 : 商品:商品编号
配送  -->  门店 : 门店:门店编号
采购  -->  供应商 : 供应商:供应商编号
采购  -->  商品 : 商品:商品编号
采购  -->  采购员 : 采购员:编号
销售  -->  商品 : 商品:商品编号
销售  -->  营业员 : 营业员:编号
销售  -->  门店 : 门店:门店编号
