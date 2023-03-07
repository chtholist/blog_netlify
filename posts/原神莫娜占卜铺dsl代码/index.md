# 【原神】莫娜占卜铺-DSL代码




问题：Mona的计算器使用其内置算法，如“期望伤害”，“最高伤害”的单技能计算。无法计算某些流派和手法的伤害（比如烈绽放烟绯，自定义蒸发和绽放的比例）

解决：自己写DSL代码

参考：综合了以下NGA帖子 + Mona的github文档

1. [分享用于莫娜占卜铺的六命/低命/蒸发夜兰DSL代码](https://nga.178.com/read.php?tid=35059588)
2. [如何为了深渊配置自己的圣遗物？以莫娜占卜铺为例](https://nga.178.com/read.php?tid=35249442)
3. [圣遗物配装工具莫娜占卜铺从入门到精](https://nga.178.com/read.php?tid=35579586)
4. [莫娜占卜铺](https://www.mona-uranai.com/)
5. [GitHub - wormtql/genshin_artifact: 莫娜占卜铺 | 原神 | 圣遗物搭配 | 圣遗物潜力。多方向圣遗物自动搭配，多方向圣遗物潜力与评分, Genshin Impact](https://github.com/wormtql/genshin_artifact)

### 使用方式

DSL代码有两种使用方式：

1. 在Mona的【计算-计算器】中进行配装
   1. 查具体角色的数据
   2. 在【MONA-DSL】中列出所有要考虑的`dmg`
   3. `result = 所有dmg的加权和`
   4. 代码框底下的蓝条`[与最大计算伤害的百分比]/[该组圣遗物计算伤害]`

1. 在Mona的【附加功能-Playground】中计算伤害
   1. 查具体角色的数据
   2. 列出所有要考虑的`dmg`
   3. `print()`打印出来



### 普通&增幅伤害

普通元素/物理伤害、融化/蒸发伤害等

注意：普通伤害有2级属性，都要写

```
dmg [伤害名] = [角色名].[技能名]([对应状态])
```

1级属性:

1. `[伤害名].normal`  普通
2. `[伤害名].melt`  融化
3. `[伤害名].vaporize`  蒸发
4. `[伤害名].aggravate`  雷激化
5. `[伤害名].spread`  蔓激化
6. `.bloom`  绽放
7. `hyperbloom`  超绽放
8. `burgeon`  烈绽放

2级属性：

1. `e`期望伤害
2. `c`暴击伤害
3. `n`非暴击伤害

例：神里，普攻第1段，发生在冲刺之后，融化，期望伤害

```
dmg a = KamisatoAyaka.Normal1({ after_dash: true })
print(a.melt.e)
```



### 剧变反应

聚变反应：扩散、感电、超载、超导、碎冰

路径：`mona_core/src/damage/transformative_damage.rs`

```
dmg = [角色名].transformative
```

属性：

- `swirl_cryo` 冰扩散
- `swirl_pyro` 火扩散
- `swirl_electro` 雷扩散
- `swirl_hydro` 水扩散
- `overload` 超载
- `electro_charged` 感电
- `super_conduct` 超导
- `shatter` 碎冰
- `crystallize`  结晶盾



### 属性声明

直接取得角色的实时面板属性，例如充能、元素精通等（角色面板在内部采用DAG+懒更新的方式）

```
prop [变量名] = [角色名].[属性名]
```

属性：

- `recharge` 充能
- `atk` 攻击
- `hp` 生命
- `def` 防御
- `em` 精通
- `crit0` 暴击率（不含特定技能和特定元素伤害的加成）
- `cd0` 暴击伤害（不含特定技能和特定元素伤害的加成）
- `heal` 治疗加成

注意：

buff的覆盖没法调整，都是按全覆盖来算

获取属性我能想到的一种用法是用来转化成目标函数的一部分，比如说钟离我希望如果生命够高，那伤害低一些就低一些，我可以接受用2伤害换1血量的话，那就可以把生命值x2加到目标函数里，第二种用法就是覆盖率，比如说我知道某几次攻击是在某个攻击力buff消失后，那么我可以给那几次攻击乘上“减去buff的面板攻击力/面板攻击力”这个系数，暴击爆伤都有类似的转换方法，但是增伤不行，因为增伤在dsl里没法获取



### 内置函数

1. `print()`
2. `type()`
3. `max()`  `min()`



### 查询具体角色

具体角色的数据：[mona_core/src/character/characters](https://github.com/wormtql/genshin_artifact/tree/main/mona_core/src/character/characters)

元素：`anemo`风、`cryo`冰、`Pyro`火、`Hydro`水、`Electro`雷、`Dendro`草

搜索`CharacterName` —— CharacterName::RaidenShogun

搜索`skillmap` —— 技能名，和网站上的技能名是对应的（通用技能的中文名不会写出来）

搜索`ItemConfig` —— 特殊的状态，比如烟绯的灼灼、雷神的环

> 例：雷神大招，在其e技能下，愿力层数57
>
> ```
> dmg q = RaidenShogun.Q1({under_e:true,resolve_stack:57})
> ```



### 例子

#### 夜兰

```Apache
// 列出所有伤害
dmg Q1=Yelan.Q1 //Q开启伤害
dmg Q2=Yelan.Q2 //Q协同伤害(每跳)
dmg E=Yelan.E1 //E伤害
dmg C6=Yelan.Charged3C6 //C6破局矢伤害
dmg Z=Yelan.Charged3 //破局矢伤害

// result=所有伤害的加权和
result=Q1.normal.e +
Q1.normal.e * 0.9014 * 7 + //利用Q开启伤害的90.14%倍模拟C2的14%生命倍率水箭，需将Q设置为13级才是此比例，10级比例为1.0646
Q2.normal.e * 3 * 16 +
E.normal.e * 2 +
C6.normal.e * 5 +
Z.normal.e *2 //两次手动重击的破局矢，依据自己的手法不同可以去掉，大约占总倍率的4%
```

#### 神里

```Rust
// 1 设置Q最大
dmg Q = KamisatoAyaka.Q1({after_dash:true}) //冲刺天赋触发后
result = Q.normal.e
// 2 大招期望输出融化和普通各占一半，且伤害发生在冲刺之后（天赋18%冰伤加成）
dmg q = KamisatoAyaka.Q1({ after_dash: true })
normal = q.normal.e
melt = q.melt.e
result = normal + melt
```

#### 甘雨

名字：Ganyu

技能：

```Rust
pub enum GanyuDamageEnum {
    Normal1,
    Normal2,
    Normal3,
    Normal4,
    Normal5,
    Normal6,
    Charged1,    // 瞄准
    Charged2,    // 一段蓄力
    Charged3,    // 霜华失 - 命中
    Charged4,    // 霜华失 - 绽放
    Plunging1,
    Plunging2,
    Plunging3,
    E1,
    Q1
}
```

特殊状态：

1. `talent1_rate`  唯此一心
2. `talent2_rate`  天地交泰

纯重击

```Apache
dmg b = Ganyu.Charged3
result = b.normal.e
```

#### 烟绯

名字：Yanfei

技能：

```Rust
pub enum YanfeiDamageEnum {
    Normal1,       // 一段伤害
    Normal2,       // 二段伤害
    Normal3,       // 三段伤害
    Charged1,      // 重击-无印
    Charged2,      // 重击-1层
    Charged3,      // 重击-2层
    Charged4,      // 重击-3层
    Charged5,      // 重击-4层
    DmgTalent2,    // 天赋2-额外
    Plunging1,     // 下坠期间
    Plunging2,     // 低空坠地
    Plunging3,     // 高空坠地
    E1,            // E技能 - 丹书立约
    Q1,            // Q技能 - 凭此结契
}
```

特殊状态：

1. `after_q`  灼灼

