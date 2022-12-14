---
title: "教务系统使用指南（v1.2.0）"
date: 2021-12-11T21:06:15+08:00
lastmod: 2021-12-11T21:06:15+08:00
author: ["SuperEggs"]
tags:
- laravel
---


## 一、 系统介绍

「EMS 教务管理系统」是根据风华国韵艺考中心定制化的一款教务管理软件，通过对教务数据的高度整合，让繁琐的课时统计变得简单高效，让课时追溯变得易手可得；系统的通知功能更是拉近了机构与学员的距离，欢迎新同学入学的通知、操作课时的通知、课时不足的预警通知，都将通过短信的形式传达的学员及其家长的手机里。分校区管理，降低了机构开设新校区的系统使用及学习成本，一个系统全部搞定。

## 二、 开始使用

###  入门篇

![image-20210301140910680](https://i.loli.net/2021/05/18/aQUITugJkq271zw.png)

系统地址：xxxxxxx

账号：注册的手机号

初始密码：123456  （登录后及时修改密码）



![image-20210301141129580](https://i.loli.net/2021/05/18/EjCrGWXvSQH8idL.png)

> 1. 排课快捷按钮
> 2. 版本号（可能不一样），点击可预览系统更新日志
> 3. 所在校区
> 4. 账号设置：修改密码，修改头像，修改昵称等
> 5. 夜间模式开关



![image-20210301141441952](https://i.loli.net/2021/05/18/ZVQdhcHfjMuDqnr.png)

> 1.功能菜单栏
>
> 2.快捷功能栏
>
> 3.学生剩余课时不足 5 节预警展示

###  校区篇（高级权限可见）

![image-20210301142052025](https://i.loli.net/2021/05/18/JcD3O74lq2M1pjd.png)

- 校区之间数据相互隔离，顶部状态栏会显示当前所属校区，[全部校区]则可以管理全部数据。

- 校区不支持也不能进行删除操作，目前只支持新增、编辑操作。
- 校区关闭后，该校区的所有管理员将不能登录系统。



### 老师篇

![image-20210301142330153](https://i.loli.net/2021/05/18/2VUTn9fvQMuN5p8.png)

- 老师管理支持新增、编辑操作，不支持删除。
- 老师创建时，需要选择所属校区，选定校区后将只能在所属校区服务。
- 老师离职，将老师状态关闭即可，老师状态关闭后，将不会在其他地方检索到。
- 编辑老师信息（姓名），将会影响到历史数据，谨慎操作。

老师数据导入：待补充

老师数据导出：待补充

### 教室篇

![image-20210301142959861](https://i.loli.net/2021/05/18/MFuOqYUISLn8cgi.png)

- 教室的信息主要用于录课操作，记录学生上课地点，便于后期数据分析。
- 教室管理支持新增、编辑操作，不支持删除。
- 如遇到教室不可用时，将教室状态关闭即可，教室状态关闭后，将不会在其他地方检索到。
- 教室数据编辑（教室名称），将会影响到历史录课数据，谨慎操作。



### 科目管理

![image-20210301143320372](https://i.loli.net/2021/05/18/lbgFyfpKEx49ndP.png)

- 科目管理支持新增，不支持编辑和删除。
- 如果科目不可用时，将科目状态关闭即可，科目状态关闭后，将不会在其他地方检索到。
- 科目类型分为一对一、集体课两种，目前系统只支持一对一的录课操作；一对一的科目可以计算课时，集体课主要用于记录结课时间。
- 科目名称命名尽量简短明了，万不可重复。

### 学生篇

![image-20210908000548892](https://i.loli.net/2021/09/08/pzFU3CE2xY1QMq9.png)

> - 位置 1：填写毕业时间的学生属于毕业学生，反之属于在校学生
>
> - 点击学生姓名可以进入到学生个人信息页
>
> - 学生数据删除，将会把该学员关联的所有数据全部清空，默认只有超级管理员有权限。

#### 新增学生

基本信息：

![image-20210301144221776](https://i.loli.net/2021/05/18/QOLBPFUG5Tzmsi9.png)![image-20210301144230854](https://i.loli.net/2021/05/18/soilhHAEKUYBNW9.png)

合同信息：

![image-20210301144326364](https://i.loli.net/2021/05/18/WmrofXh7ugGDJkq.png)

- 合同附件仅支持格式：pdf，png，jpg
- 合同附件建议上传 pdf 格式，方便在线预览。
- 如果是多张图片文件，点击「图片转 pdf 工具」，合成一个 pdf 文件再上传。
- 合同附件最大支持 10 M，如果大于该限制，请自行压缩再上传。

#### 学生信息展示

##### 基本信息

![image-20210907235754572](https://i.loli.net/2021/09/07/1HOh2BUrMdTt9pe.png)

>年度：学生所在学年
>
>性别：女、男
>
>联系方式：唯一
>
>咨询老师：关联的咨询老师
>
>地区：省市区
>
>合同副本：点击可在新窗口预览
>
>校区：学生所属校区
>
>课程类型：单报、自选套餐、暑假预科、寒假预科
>
>入学时间：学生入学时间
>
>毕业时间：学生的毕业时间，未毕业则为空
>
>身份证号：学生的身份证号
>
>班级：学生所在班级名称
>
>学生绑定码：点击获取学生绑定微信身份的小程序码，有效期为 2 个小时
>
>监护人绑定码：点击获取学生监护人绑定微信身份的小程序码，有效期为 2 个小时
>
>备注：学生备注描述

![image-20210907235827092](https://i.loli.net/2021/09/07/S1NQPAOjuGMTn9g.png)

> 1. 合同副本在线预览
>
> 2. 调整该学生课程（新购、转移、续费）
>
> 3. 修改该课程的授课老师
>
> 4. 关闭该课程，课程关闭后将不能录课，课程操作。
>
> 5. 弹窗展示课程流转日志，追溯课程课时
>
>    ![image-20210301145254180](https://i.loli.net/2021/05/18/5XpA4OoRkiDWHay.png)

###  课程篇

> 待完善

### 录课篇（课程日志）

![image-20210301145617225](https://i.loli.net/2021/05/18/x3HlGyzYRkWoB5q.png)

> 1. 课程授课老师。
> 2. 录入的上课日期，可操作时间段见下表：
>
> | 操作的时间范围*         | 可录入的时间范围      |
> | ----------------------- | --------------------- |
> | 当天 12:00 - 当天 24:00 | 当天 8:00 - 当天12:00 |
> | 当天 8:00 - 当天12:00   | 昨天 8:00 - 昨天24:00 |
>
> 3. 学生数据，可一次创建多条，创建顺序：学生姓名-课程-教室-数量；
> 4. 学生的上课数量可选【0.5、1、2】
> 5. 上课时间自由选择，非必填项。



#### 课程记录回滚

![image-20210301163145866](https://i.loli.net/2021/05/18/CJQyfpTWid6ES4s.png)

- 课程回滚操作，会将课程日志删除，录入的上课数量回滚到原有的课程数量上。
- 课程回滚操作，只能原操作者有操作权限。

### 课程操作篇

![image-20210301150522816](https://i.loli.net/2021/05/18/H7jXng6R5hrWIaB.png)

#### 新购（第一次购买该科目课程）

![image-20210301150548920](https://i.loli.net/2021/05/18/ae7X1FZisbocYOV.png)

- 一对一类型，选择新购数量，该新购数量将作为统计课时的重要依据

![image-20210301150718507](https://i.loli.net/2021/05/18/uSKmLnz62YabFpd.png)

- 集体课类型，选择结课时间，结课时间是该课程的最后期限，结课时间提交后不允许再调整。

#### 转课（一对一转一对一，集体课转一对一）

![image-20210301151051258](https://i.loli.net/2021/05/18/OZuBdRprPqa8yn2.png)

> 1. 转出课程可选为状态开启的，并且有剩余课时的一对一或集体课课程
> 2. 转出数量必须大于转出课程的剩余课时，否则将传出失败
> 3. 转入课程可选为状态开启的一对一课程，不支持集体课课程
> 4. 转入数量默认与转出数量一致，转入转出比系统不做限制。转入数量与转出数量最小为 1 节
> 5. 转课描述。非必填项。
> 6. 转课单附件，这里支持图片、pdf 多种格式。优先使用图片，方便在线预览。

#### 续费（已购买的课程）

![image-20210908001631379](https://i.loli.net/2021/09/08/dSWPXAD3hVU2pcF.png)

- 操作课程只能为状态开启的课程
- 续费课时量最小为 1；
- 续费合同支持图片、pdf 多种格式。优先使用图片，方便在线预览。

![image-20210908001643467](https://i.loli.net/2021/09/08/dF1b3oGHslx8qEv.png)

- 集体课续费结课时间必须大于原结课时间
- 续费合同支持图片、pdf 多种格式。优先使用图片，方便在线预览。



#### 课程操作回滚

![image-20210301152218241](https://i.loli.net/2021/05/18/y6YbtnopTKCEvO7.png)

- 课程操作日志中，可以对错误的课程操作进行回滚
- 只能对自己创建的课程操作进行回滚
- 课程操作回滚将删除操作课程记录，回滚操作的课时
  - 新购操作：若新购的课程已进行了其他的课程操作（转课、续费、录课消课），则不支持回滚操作。
  - 转课操作：若当初转入的课程，现在剩余的课时量小于当初转入的课程数量，则不支持回滚操作。
  - 续费操作：
    - 一对一：若当初续费的课程，现在剩余的课时量小于当初续费的课程数量，则不支持回滚操作。
    - 集体课：暂不限制

### 系统配置（高级管理员可见）

#### 基本配置

![image-20210908000956418](https://i.loli.net/2021/09/08/i5fN6C23MdLFHEA.png)

- 录课时间限制
  - 关闭后，录课时的上课日期可以任意选择。
  - 开启后，只能提交当天以及昨天的课程记录；可配置昨天的课程于今日最晚的录入时间，
- 短信验证码频率：多少秒可以发送的一条验证码
- 短信验证码限制(次/每天)：每人每天可以发送多少条验证码

#### 课程操作配置

![image-20210908001122778](https://i.loli.net/2021/09/08/BszP61jok4XGHCZ.png)

- 课程续费操作附件必填
- 课程转移操作附件必填

#### 上传文件管理

![image-20210301155125727](https://i.loli.net/2021/05/18/qBtrv1CDANaiPK2.png)

- 可以对上传时用到的电子表格进行删除。

### 班级管理

班级列表

![image-20210903235848076](https://i.loli.net/2021/09/03/s9PKVxntUgedoT6.png)

新建班级：

![image-20210904000105799](https://i.loli.net/2021/09/04/FMXckoCI6fQ5GhO.png)

> 1. 班级名称，同一校区不允许重复
> 2. 校区配置
> 3. 设置班主任，每个班级只能有一个班主任，但是一个人可以担任多个班级的班主任。
> 4. 状态值，关闭后，班级不可用（谨慎操作）

### 学生请假篇

#### （必须）学生及家长关联微信

![image-20220308225301581](https://tva1.sinaimg.cn/large/e6c9d24ely1h02vpar9p1j21ww0mwtbj.jpg)

> 1. 点击学生绑定码按钮后，会弹出一个小程序绑定码，请指导学生进行扫描绑定身份，绑定的微信将作为今后学生发起请假的指定唯一微信。小程序绑定码有效期两个小时，请在有效期内操作。
> 2. 同上操作，指导学生家长进行微信身份绑定。家长目前仅支持一个人绑定。该绑定的家长微信将作为学生请假审批的唯一指定微信，同时在微信身份绑定过程中，绑定的手机号，将作为短信通知指定号码，暂不支持修改。

#### （必须）班主任微信绑定

![image-20220308230339606](https://tva1.sinaimg.cn/large/e6c9d24ely1h02vpg2bgsj22380siq8k.jpg)

>  学生请假必须项，点击绑定码按钮，扫描弹出小程序身份绑定码，进行班主任身份绑定，该微信作为学生请假的班主任阶段指定唯一微信，绑定的手机号，将作为短信通知指定号码，每次学生提交请假后，并且家长通过后，会有短信通知。手机号码暂不支持修改。

#### 学生请假明细

![image-20220308225954643](https://tva1.sinaimg.cn/large/e6c9d24ely1h02vpjlxjlj222w0moq6e.jpg)

![image-20220308230047322](https://tva1.sinaimg.cn/large/e6c9d24ely1h02vpnfxdbj21rb0u0mzs.jpg)

在后台 请假管理 → 请假明细中，可查看学生请假列表。

### 监护人管理

![image-20210907235014231](https://i.loli.net/2021/09/07/MaPX5HCiAQTE3Ff.png)

- 监护人暂不支持后台手动新增，开放时间待定
- 监护人只能通过扫描学生绑定家长小程序码，进行绑定创建
- 监护人手机号唯一且必填

### 微信用户管理

![image-20210907235233723](https://i.loli.net/2021/09/07/dSA1zLPIU7hkDaf.png)

- 微信用户不支持后台创建
- 微信用户身份类型分为：学生、监护人、班主任、普通用户
- 微信用户操作：
  - 解除关联：解除当前微信用户与后台身份的关联，敏感操作，谨慎使用。



### 工单管理

#### 提工单

工单入口：

- 快捷入口：系统右上角「提工单」按钮：<img src="https://s2.loli.net/2021/12/09/dsq8BTyAU4QvELJ.png" alt="image-20211208234722963" style="zoom:40%;" />
- 菜单入口：工单管理 → 新增

填写内容：

![image-20211208234948592](https://s2.loli.net/2021/12/08/xjcoqY3nsCePIRW.png)![image-20211208235014358](https://s2.loli.net/2021/12/09/noLMcNS4bkQiWCG.png)

- 处理人：目前固定为 超级管理员。
- 优先级：默认为「中」（对应普通），请合理选择处理等级，随意选择只会试工单排名降低。
- 标题：优秀的标题要能用一句话表达需求，6个字至35个字。
  - 不合格标题示例：<img src="https://s2.loli.net/2021/12/09/Rhf278GSqIxAMVm.png" alt="image-20211208235531703" style="zoom:50%;" />
- 工单内容：根据编辑框中的提示填写即可，可上传图片，这里要尽量详细，暂无字数限制。
- 截止时间：期望的工单的完成时间，非必填。请填写合理的时间，不清楚请留空。
- 附件：用户辅助描述工单需求，支持多图，单图小于 10M。

#### 工单详情

![image-20211209231539798](https://s2.loli.net/2021/12/09/YI5Njt12SvQniGP.png)

![image-20211209231631985](https://s2.loli.net/2021/12/09/M3xKlkiB6gVdzJu.png)

> 1. 工单标题
> 2. 工单基础信息
> 3. 工单内容
> 4. 工单附件（目前仅支持图片格式）
> 5. 复制当前工单链接。
> 6. 工单操作：删除工单。（仅管理员及创建者有权限删除）
> 7. 工单状态修改。
> 8. 工单详细信息。
> 9. 工单动态列表
> 10. 工单评论，字数 500 字以内。支持图片。（评论框如果没有出来,请尝试刷新页面）
> 11. 评论列表
> 12. 回复指定评论

#### 工单动态提醒

![image-20211209231119595](https://s2.loli.net/2021/12/09/eAHc85ETaIobKu6.png)

> 1. 消息提醒图标。
> 2. 点击可查看相关工单。
> 3. 点击全部已读，关闭工单动态消息。

#### 工单验收

当工单任务完成后，工单处理人会将工单状态修改为<img src="https://s2.loli.net/2021/12/09/GouxqSrPwND2sEj.png" alt="image-20211209232237665" style="zoom:50%;" />，此时，就需要工单创建人，验收测试，如果没有问题则将工单状态修改为「关闭」，如果发现依旧存在问题，则将状态修改为「再次打开」，并将问题评论在下方，以便处理人后续处理。

## Todo：

- [ ] 班级增加学年配置
- [ ] 工单动态短信提醒





## 常见问题

### 我进入 xxx，显示「无权访问」？

![image-20210301153034871](https://i.loli.net/2021/05/18/fUAKac3edEWi65D.png)

- 该菜单栏目未公开，联系管理员获取权限。

### 后台找不到 「XXX」菜单，怎么回事？

-  没有权限，联系管理员获取权限。

### 登录失败，显示「校区已关闭」？

- 所属校区已被管理员关闭，请联系管理员。

### 课程操作录入错误怎么办？录课信息填写错误怎么办？

- 请阅读`课程日志回滚`、`课程操作日志回滚`。

### 遇到其他任何错误：

- 及时截图反馈到微信群。

## 更新日志

### 1.1.5 「2021-07-01」

#### 修复 bug

- 修复课程操作时出现课时错误的 bug

#### 功能新增

- 增加学生毕业管理

#### 功能优化

- 优化课程操作描述,增加课时操作后最新数据
- 优化学生页面显示,展示更多数据信息

### 1.1.4 「2021-06-07」

#### 修复 bug

- 修复排课操作时出现集体课的 bug

### 1.1.3 「2021-05-19」

#### 功能新增

- 新增教室名称搜索
- 新增科目名称搜索

#### 功能优化
- 优化老师校区管理,老师可跨校区操作
- 增加排课课程数量必填设置

### 1.1.2 「2021-05-12」

#### 功能优化

- 优化课程操作回滚逻辑
- 优化操作提示
- 优化学生头像显示
- 升级 Dcat 版本至:2.0.24

### 1.1.1 「2021-05-02」

#### 功能优化

- 完善查询课程操作记录的权限问题
- 优化删除学生逻辑

### 1.1.0 「2021-03-17」

#### 功能新增

- 完善后台权限
- 新增管理员状态配置

#### 功能优化

- 优化数据筛选,添加操作人查找
- 其他优化

### 1.0.9 「2021-03-10」

#### 功能新增

- 新增学生列表地区筛选功能
- 新增系统日志插件

#### 功能优化

- 优化排课页面,课程明细字段翻译问题
- 升级到 dcat v2.0.20-beta

### 1.0.8 「2021-03-01」

#### 功能新增

- 新增课程操作时间轴
- 新增续费课程合同上传
- 新增课程日志的筛选,
- 新增老师的筛选
- 新增课程操作回滚,限制回滚课程操作的只能是操作者本人
- 增加后台配置,排课时间限制可通过后台配置修改

#### 功能优化

- 支持 0.5 节的课时操作.
- 

#### 修复 bug

- 修复排课时间限制 bug

### 1.0.7 「2021-02-26」

#### 功能新增

- 新增系统更新日志
- 新增学生创建后后向学生发送短信提醒
- 新增课程操作日志模态框

#### 功能优化

- 系统性能优化

### 1.0.6 「2021-02-25」

#### 功能新增

- 顶部增加校区提示
- 新增系统配置项(未上线)
- 新增系统异常邮件通知
- 新增购课备注
- 支持集体课转一对一
- 新增学生导出

#### 功能优化

- 学生删除连带删除课程信息
- 优化排课逻辑
- 优化操作课程日志事件
- 禁止老师,教室等数据删除
- 校区权限优化
- 修复登录时异常,由于头部变量为定义的问题
- 增加老师分校区管理,严格执行 12 点策略

### 1.0.5 「2021-02-20」

#### 功能新增

- 新增分校区管理体系,自此支持分校区管理
- 新增后台扩展插件文件
- 新增管理员字段:手机号,邮箱

#### 功能优化

- 描述’增加’修改为’续费’
- 首页只提示一对一的课程
- 优化集体课显示

### 1.0.4 「2021-02-08」

#### 功能新增

- 新增性能检测插件
- 增加短信接口
- 新增课程分类,以此区分集体课一对一

#### 功能优化

- 升级 dcat
- 优化课程编辑操作
- 优化排课,只取有课时的学生数据
- 优化集体课展示

### 1.0.3 「2021-02-03」

#### 功能新增

- 增加学生地区管理

#### bug修复

- 修复课程不能为 0 的 bug
- 修复 select bug
- 修复地区关联为空时,报错的 bug
- 修复首页不能预警负数bug,修复部分其他 bug

#### 功能优化

- 地区关系优化

### 1.0.2 「2021-01-29」

#### 功能新增

- 顶部添加排课快捷入口

#### 功能优化

- 优化学生详情页
- 优化教室管理
- 优化课程操作日志

### 1.0.1 「2021-01-26」

#### 功能新增

- 增加学生学号
- 增加 dep 部署配置文件
- 新增排课日志

#### 功能优化

- 优化 admin 配置
- 优化排课操作

### 1.0.0 「2021-01-22」

#### 功能新增

- 学生信息管理
- 老师信息管理
- 教室管理
- 课程管理
- 新增排课操作

### 0.0.0 「2021-1-19」

### 项目启动