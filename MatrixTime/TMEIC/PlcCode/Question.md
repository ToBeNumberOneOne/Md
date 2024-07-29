
## 疑问

### 1 LandSied Crane 和 WaterSide Crane 

在参数设置和程序逻辑中有一定的区别，具体的物理特性和作业流程逻辑存在哪些差异

- 登机口的门方向
- 海测对AGV或AGV伴侣作业，陆侧对外集卡作业

### 2 




### 3 PME编程软件的使用，硬件组态，逻辑编程

交叉引用比较麻烦，用绝对地址编程，需要对程序理解才能快速定位到具体功能快。

部分功能块加密，Clearanlce logic, 内部功能块：SkewFilt_EXE
部分功能块不能打开：Skew C filter

LSA：Land side automation drop off enable

VMR: Vernier motion regulator

ALS: Auto landing system


AUTOMATIC LANDING SYSTEM(ALS) 

- Bit 1 = ALS enable 
- Bit 2 = Enable memorisation
- Bit 3 = Report Memorised values
- Bit 4 = Clear Memory
- Bit 5 = Spreader Position Valid
- Bit 6 = Spreader speed Valid
- Bit 7 = Unplanned Manual Intervention
- Bit 8 = Planned Manual Intervention
- Bit 9 = Crane Reset by operator
- Bit 10 = Set landing target to ideal
- Bit 11 = Set landing target position to ALSexternal1

LSA_DO_ActLS : Landside automation auto drop off active

LSA_PU_AcLS  : Landside automation auto pick up active

ACL : Anti cillison



