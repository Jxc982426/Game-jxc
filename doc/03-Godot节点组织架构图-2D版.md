# 03-暗黑挂机游戏 - Godot 节点组织架构图 (2D 版)

## 📋 文档概述

本文档详细描述了游戏场景的节点组织结构，包括主场景、玩家场景、敌人场景、UI 场景等的完整节点树结构。适合在 Godot 编辑器中参考创建。

**重要：** 本版本为纯 2D 架构，所有节点均为 2D 类型。

---

## 一、主场景（Main.tscn）

### 1.1 完整节点树

```
Main (Node2D) [脚本：Main.gd]
│
├── Camera2D (Camera2D)
│   └── [主摄像机：2D 俯视角，跟随玩家移动]
│
├── GameManager (Node) [Autoload: GameManager]
│   │   # 游戏总管理器
│   │
│   ├── EventBus (Node) [Autoload: EventBus]
│   │   └── [全局事件信号总线]
│   │
│   ├── SaveSystem (Node) [Autoload: SaveSystem]
│   │   └── [存档读写管理]
│   │
│   └── AudioController (Node)
│       ├── BGMPlayer (AudioStreamPlayer)
│       │   └── [背景音乐循环]
│       │
│       └── SFXController (Node)
│           └── [音效池管理]
│
├── PlayerManager (Node) [Autoload: PlayerManager]
│   │   # 玩家管理器
│   │
│   └── Player (CharacterBody2D)
│       │   # 玩家实例（2D 刚体）
│       │
│       ├── CollisionShape2D (CollisionShape2D)
│       │   └── [圆形或胶囊形碰撞体]
│       │
│       ├── Sprite (Sprite2D)
│       │   └── [玩家精灵渲染]
│       │
│       ├── AnimatedSprite2D (AnimatedSprite2D)
│       │   └── [玩家动画精灵]
│       │
│       ├── NavigationAgent2D (NavigationAgent2D)
│       │   └── [2D 寻路代理]
│       │
│       ├── RayCast2D (RayCast2D)
│       │   │   # 攻击命中检测射线
│       │   │
│       │   └── target_position: Vector2(0, -5)
│       │
│       ├── Area2D (LootRange)
│       │   │   # 自动拾取范围
│       │   │
│       │   └── CollisionShape2D (CollisionShape2D)
│       │       └── shape: CircleShape2D (半径 50 像素)
│       │
│       ├── Stats (Node)
│       │   │   # 属性组件
│       │   │
│       │   ├── BaseStats (Resource)
│       │   │   └── [基础属性数据]
│       │   │
│       │   └── DerivedStats (Resource)
│       │       └── [衍生属性数据]
│       │
│       ├── Skills (Node)
│       │   │   # 技能组件
│       │   │
│       │   ├── SkillSlot1 (Skill Resource)
│       │   ├── SkillSlot2 (Skill Resource)
│       │   ├── SkillSlot3 (Skill Resource)
│       │   └── SkillSlot4 (Skill Resource)
│       │
│       └── Equipment (Node)
│           │   # 装备组件
│           │
│           ├── Head (Equipment Resource)
│           ├── Body (Equipment Resource)
│           ├── Weapon (Equipment Resource)
│           ├── Accessory1 (Equipment Resource)
│           └── Accessory2 (Equipment Resource)
│
├── EnemyManager (Node) [Autoload: EnemyManager]
│   │   # 敌人生成与管理
│   │
│   └── EnemySpawner (Node2D)
│       │   # 怪物生成器
│       │
│       ├── SpawnPoint1 (Marker2D)
│       │   └── [2D 生成点 1]
│       │
│       ├── SpawnPoint2 (Marker2D)
│       │   └── [2D 生成点 2]
│       │
│       ├── SpawnPoint3 (Marker2D)
│       │   └── [2D 生成点 3]
│       │
│       └── Timer (Timer)
│           └── [定时生成计时器]
│
├── ItemManager (Node) [Autoload: ItemManager]
│   │   # 物品管理器
│   │
│   ├── DroppedItems (Node2D)
│   │   │   # 2D 掉落物品容器
│   │   │
│   │   └── [动态生成的物品实例...]
│   │
│   └── ItemDatabase (Resource)
│       └── [物品数据库配置]
│
├── UIManager (CanvasLayer)
│   │   # UI 管理层（所有 UI 的父节点）
│   │
│   ├── HUD (Control)
│   │   │   # 抬头显示（始终可见）
│   │   │
│   │   ├── TopBar (HBoxContainer)
│   │   │   ├── HealthOrb (TextureProgressBar)
│   │   │   │   └── [红色球形血条]
│   │   │   │
│   │   │   ├── ManaOrb (TextureProgressBar)
│   │   │   │   └── [蓝色球形蓝条]
│   │   │   │
│   │   │   └── ExpBar (ProgressBar)
│   │   │       └── [经验值条]
│   │   │
│   │   ├── LevelLabel (Label)
│   │   │   └── [等级数字显示]
│   │   │
│   │   ├── SkillBar (HBoxContainer)
│   │   │   │   # 技能快捷栏
│   │   │   │
│   │   │   ├── SkillButton1 (TextureButton)
│   │   │   │   ├── Icon (TextureRect)
│   │   │   │   ├── CooldownOverlay (ColorRect)
│   │   │   │   └── KeyHint (Label) [1/Q]
│   │   │   │
│   │   │   ├── SkillButton2 (TextureButton)
│   │   │   ├── SkillButton3 (TextureButton)
│   │   │   └── SkillButton4 (TextureButton)
│   │   │
│   │   ├── GoldLabel (Label)
│   │   │   └── [金币数量显示]
│   │   │
│   │   └── DamageMeter (VBoxContainer)
│   │       └── [伤害统计面板（可选）]
│   │
│   ├── InventoryWindow (Window)
│   │   │   # 背包窗口（可关闭）
│   │   │
│   │   ├── WindowTitle (Label)
│   │   │   └── ["背包"]
│   │   │
│   │   ├── GridContainer (GridContainer)
│   │   │   │   # 物品格子容器
│   │   │   │
│   │   │   ├── Slot1 (Panel)
│   │   │   │   ├── ItemIcon (TextureRect)
│   │   │   │   └── QuantityLabel (Label)
│   │   │   │
│   │   │   ├── Slot2 (Panel)
│   │   │   ├── ... (共 30-50 个格子)
│   │   │   └── Slot50 (Panel)
│   │   │
│   │   └── CloseButton (Button)
│   │
│   ├── CharacterWindow (Window)
│   │   │   # 角色面板
│   │   │
│   │   ├── PaperDoll (Control)
│   │   │   │   # 纸娃娃系统（展示装备）
│   │   │   │
│   │   │   ├── HeadSlot (TextureButton)
│   │   │   ├── BodySlot (TextureButton)
│   │   │   ├── WeaponSlot (TextureButton)
│   │   │   ├── Accessory1Slot (TextureButton)
│   │   │   └── Accessory2Slot (TextureButton)
│   │   │
│   │   ├── StatsPanel (VBoxContainer)
│   │   │   │   # 属性列表
│   │   │   │
│   │   │   ├── StrengthLabel (Label)
│   │   │   ├── Agility_label (Label)
│   │   │   ├── Intelligence_label (Label)
│   │   │   ├── Vitality_label (Label)
│   │   │   ├── Damage_label (Label)
│   │   │   ├── Defense_label (Label)
│   │   │   └── ... (其他属性)
│   │   │
│   │   └── AttributePoints (HBoxContainer)
│   │       └── [属性点分配按钮们]
│   │
│   ├── HangupConfigWindow (Window)
│   │   │   # 挂机配置窗口
│   │   │
│   │   ├── SkillPriorityGroup (VBoxContainer)
│   │   │   │   # 技能优先级
│   │   │   │
│   │   │   ├── PriorityLabel (Label)
│   │   │   ├── SkillOrderList (ItemList)
│   │   │   └── [拖拽排序的技能列表]
│   │   │
│   │   ├── HealthThresholdGroup (VBoxContainer)
│   │   │   │   # 血量阈值
│   │   │   │
│   │   │   ├── UseHealthPotionCheck (CheckButton)
│   │   │   ├── HealthSlider (HSlider)
│   │   │   └── HealthValue_label (Label)
│   │   │
│   │   ├── ManaThresholdGroup (VBoxContainer)
│   │   │   └── [蓝量阈值设置，同上]
│   │   │
│   │   └── LootFilterGroup (VBoxContainer)
│   │       │   # 拾取过滤
│   │       │
│   │       ├── PickCommonCheck (CheckButton)
│   │       ├── PickMagicCheck (CheckButton)
│   │       ├── PickRareCheck (CheckButton)
│   │       └── PickLegendaryCheck (CheckButton)
│   │
│   ├── QuestTracker (Panel)
│   │   │   # 任务追踪（小地图旁）
│   │   │
│   │   └── QuestList (VBoxContainer)
│   │       └── [当前任务列表]
│   │
│   ├── MinimapContainer (Panel)
│   │   │   # 小地图容器
│   │   │
│   │   ├── MinimapViewport (SubViewport)
│   │   │   └── [小地图渲染视口]
│   │   │
│   │   └── MapName Label (Label)
│   │
│   └── SettingsWindow (Window)
│       │   # 设置窗口
│       │
│       ├── GraphicsTab (VBoxContainer)
│       ├── AudioTab (VBoxContainer)
│       ├── ControlsTab (VBoxContainer)
│       └── Tabs (TabContainer)
│
├── MinimapCamera (Camera2D)
│   │   # 小地图专用 2D 摄像机（俯视固定）
│   │
│   └── [固定俯视角度，正交投影]
│
└── DebugOverlay (Control)
    │   # 调试信息（仅开发版本）
    │
    ├── FPSLabel (Label)
    ├── EntityCountLabel (Label)
    └── MemoryLabel (Label)
```

---

## 二、玩家场景详细结构（Player.tscn）

### 2.1 独立玩家场景（可复用）

```
Player (CharacterBody2D) [脚本：Player.gd]
│
├── CollisionShape2D (CollisionShape2D)
│   └── shape: CapsuleShape2D / CircleShape2D
│
├── Sprite (Sprite2D)
│   │
│   └── texture: 玩家精灵图
│
├── AnimatedSprite2D (AnimatedSprite2D)
│   │
│   ├── SpriteFrames: 动画帧资源
│   ├── Animation: idle (待机动画)
│   ├── Animation: run (跑步动画)
│   ├── Animation: attack_01 (攻击动画 1)
│   ├── Animation: attack_02 (攻击动画 2)
│   ├── Animation: hit (受击动画)
│   └── Animation: death (死亡动画)
│
├── NavigationAgent2D (NavigationAgent2D)
│   │
│   ├── target_position: Vector2
│   ├── speed: 150.0 (像素/秒)
│   └── path_desired_distance: 5.0
│
├── RayCast2D (AttackRay)
│   │
│   ├── target_position: Vector2(0, -50)
│   └── collision_mask: 2 (敌人层)
│
├── Area2D (LootPickupRange)
│   │
│   ├── CollisionShape2D (CollisionShape2D)
│   │   └── shape: CircleShape2D (半径 50 像素)
│   │
│   └── body_entered 信号连接
│
├── AudioStreamPlayer2D (FootstepPlayer)
│   └── [脚步声播放]
│
├── GPUParticles2D (MovementDust)
│   │   # 移动时的尘土特效
│   │
│   ├── process_material: ParticleProcessMaterial
│   └── [2D 粒子设置]
│
├── Components (Node)
│   │   # 功能组件容器
│   │
│   ├── StatsComponent (Node) [脚本：StatsComponent.gd]
│   │   │
│   │   ├── BaseStats (Resource) [脚本：BaseStats.gd]
│   │   │   ├── strength: int = 10
│   │   │   ├── agility: int = 10
│   │   │   ├── intelligence: int = 10
│   │   │   └── vitality: int = 10
│   │   │
│   │   └── DerivedStats (Resource) [脚本：DerivedStats.gd]
│   │       ├── max_health: float = 100.0
│   │       ├── current_health: float = 100.0
│   │       ├── max_mana: float = 50.0
│   │       ├── current_mana: float = 50.0
│   │       ├── physical_damage: float = 10.0
│   │       ├── armor: float = 5.0
│   │       ├── crit_rate: float = 0.05
│   │       └── crit_damage: float = 1.5
│   │
│   ├── SkillComponent (Node) [脚本：SkillComponent.gd]
│   │   │
│   │   ├── Skill1 (Skill Resource)
│   │   ├── Skill2 (Skill Resource)
│   │   ├── Skill3 (Skill Resource)
│   │   └── Skill4 (Skill Resource)
│   │
│   ├── EquipmentComponent (Node) [脚本：EquipmentComponent.gd]
│   │   │
│   │   ├── EquipmentSlots (Dictionary)
│   │   │   ├── "head": null
│   │   │   ├── "body": null
│   │   │   ├── "weapon": null
│   │   │   ├── "accessory1": null
│   │   │   └── "accessory2": null
│   │   │
│   │   └── EquipBonus (Dictionary)
│   │       └── [装备加成汇总]
│   │
│   └── BuffComponent (Node) [脚本：BuffComponent.gd]
│       │
│       ├── ActiveBuffs (Array)
│       └── [BUFF 效果管理]
│
├── StateMachine (Node) [脚本：PlayerStateMachine.gd]
│   │   # 玩家状态机
│   │
│   ├── IdleState (State Resource)
│   ├── MovingState (State Resource)
│   ├── AttackingState (State Resource)
│   ├── CastingState (State Resource)
│   ├── DamagedState (State Resource)
│   └── DeadState (State Resource)
│
└── VisualEffects (Node2D)
    │   # 视觉特效容器
    │
    ├── HitEffect (GPUParticles2D)
    │   └── [受击火花 2D 粒子]
    │
    ├── LevelUpEffect (GPUParticles2D)
    │   └── [升级光环 2D 粒子]
    │
    └── Light2D (PointLight2D)
        └── [2D 点光源（可选）]
```

---

## 三、敌人场景结构（Enemy.tscn）

### 3.1 基础敌人模板

```
Enemy (CharacterBody2D) [脚本：Enemy.gd]
│
├── CollisionShape2D (CollisionShape2D)
│   └── shape: CapsuleShape2D
│
├── Sprite (Sprite2D)
│   └── texture: 怪物精灵图
│
├── AnimatedSprite2D (AnimatedSprite2D)
│   ├── Animation: idle
│   ├── Animation: walk
│   ├── Animation: run
│   ├── Animation: attack
│   ├── Animation: hit
│   └── Animation: death
│
├── NavigationAgent2D (NavigationAgent2D)
│   └── speed: 100.0
│
├── Area2D (DetectionRange)
│   │   # 侦测范围（仇恨范围）
│   │
│   ├── CollisionShape2D (CollisionShape2D)
│   │   └── shape: CircleShape2D (半径 200 像素)
│   │
│   └── body_entered 信号连接
│
├── Area2D (AttackRange)
│   │   # 攻击范围
│   │
│   ├── CollisionShape2D (CollisionShape2D)
│   │   └── shape: CircleShape2D (半径 30 像素)
│   │
│   └── body_entered 信号连接
│
├── AudioStreamPlayer2D (GroanPlayer)
│   └── [怪物叫声]
│
├── Stats (Node)
│   │
│   ├── MaxHealth: float = 100.0
│   ├── CurrentHealth: float = 100.0
│   ├── Damage: float = 15.0
│   ├── Armor: float = 3.0
│   ├── ExperienceValue: int = 50
│   └── GoldValue: int = 10
│
├── AIController (Node) [脚本：EnemyAI.gd]
│   │
│   ├── StateMachine (Node)
│   │   ├── IdleState (State)
│   │   │   └── [2D 巡逻逻辑]
│   │   ├── ChaseState (State)
│   │   │   └── [追击玩家]
│   │   ├── AttackState (State)
│   │   │   └── [攻击逻辑]
│   │   └── ReturnState (State)
│   │       └── [返回出生点]
│   │
│   └── Blackboard (Node)
│       ├── Target: NodePath
│       ├── PatrolPoints: Array
│       └── CurrentState: String
│
├── LootTable (Resource) [脚本：LootTable.gd]
│   │
│   ├── DropEntries (Array)
│   │   ├── {item_id: "potion_small", chance: 0.3, quantity_min: 1, quantity_max: 2}
│   │   ├── {item_id: "sword_common", chance: 0.05, quantity_min: 1, quantity_max: 1}
│   │   └── {item_id: "gold", chance: 1.0, quantity_min: 5, quantity_max: 15}
│   │
│   └── GoldMin: int = 5
│       GoldMax: int = 15
│
└── DeathEffect (GPUParticles2D)
    └── [死亡爆炸 2D 特效]
```

### 3.2 精英怪变体（继承自基础敌人）

```
EliteEnemy (继承自 Enemy.tscn)
│
├── [修改的属性值]
│   ├── MaxHealth: ×2.0
│   ├── Damage: ×1.5
│   ├── ExperienceValue: ×3.0
│   └── GoldValue: ×5.0
│
├── EliteAffixes (Node)
│   │   # 精英词缀
│   │
│   ├── Affix1: "Extra Health" (+50% HP)
│   ├── Affix2: "Fire Enchanted" (火焰伤害)
│   └── Affix3: "Lightning Aura" (闪电光环)
│
└── AuraEffect (Area2D)
    │   # 光环特效
    │
    ├── CollisionShape2D
    └── GPUParticles2D (元素粒子 2D)
```

### 3.3 BOSS 变体

```
BossEnemy (继承自 Enemy.tscn)
│
├── [大幅修改的属性值]
│   ├── MaxHealth: ×10.0
│   ├── Damage: ×2.0
│   ├── ExperienceValue: ×50.0
│   └── GoldValue: ×100.0
│
├── BossMechanics (Node) [脚本：BossMechanics.gd]
│   │   # BOSS 机制
│   │
│   ├── PhaseSystem (Node)
│   │   ├── Phase1: 100%-70% HP
│   │   ├── Phase2: 70%-30% HP
│   │   └── Phase3: 30%-0% HP
│   │
│   └── SpecialAttacks (Node)
│       ├── Skill1: "Ground Slam" (顺劈)
│       ├── Skill2: "Summon Minions" (召唤小怪)
│       └── Ultimate: "Enrage" (狂暴)
│
├── BossBar (Control)
│   │   # BOSS 血条（特殊 UI）
│   │
│   └── HealthBarLarge (ProgressBar)
│
└── SummonPoint (Marker2D)
    └── [召唤小怪 2D 生成点]
```

---

## 四、掉落物品场景（DroppedItem.tscn）

### 4.1 地面掉落物

```
DroppedItem (Node2D) [脚本：DroppedItem.gd]
│
├── Area2D (ItemArea)
│   │   # 物品检测区域
│   │
│   ├── CollisionShape2D (CollisionShape2D)
│   │   └── shape: CircleShape2D (半径 20 像素)
│   │
│   └── body_entered 信号连接
│
├── Sprite (Sprite2D)
│   │   # 物品 2D 精灵
│   │
│   ├── ForWeapon: sword_texture
│   ├── ForArmor: chest_texture
│   └── ForPotion: bottle_texture
│
├── PointLight2D (ItemLight)
│   │   # 物品 2D 光照（让物品更显眼）
│   │
│   └── color: 根据品质变化
│       ├── Common: White
│       ├── Magic: Blue
│       ├── Rare: Yellow
│       └── Legendary: Orange
│
├── GPUParticles2D (QualityParticles)
│   │   # 品质特效 2D 粒子
│   │
│   └── process_material: 根据品质变化颜色
│
├── FloatingText (Control)
│   │   # 漂浮文字（物品名称）
│   │
│   └── ItemName Label (RichTextLabel)
│
├── ItemData (Resource) [脚本：ItemData.gd]
│   │
│   ├── item_id: String
│   ├── item_name: String
│   ├── item_type: String
│   ├── quality: int
│   ├── stats: Dictionary
│   └── icon: Texture
│
└── Timer (DespawnTimer)
    │   # 消失计时器
    │
    └── wait_time: 300.0 (5 分钟后消失)
```

---

## 五、UI 场景详解

### 5.1 HUD 场景（HUD.tscn）

```
HUD (CanvasLayer) [脚本：HUD.gd]
│
├── TopCenter (Control)
│   │   # 顶部中央区域
│   │
│   ├── HealthOrbContainer (Control)
│   │   │   # 血球容器
│   │   │
│   │   ├── HealthOrbBack (TextureRect)
│   │   │   └── texture: 空球背景
│   │   │
│   │   ├── HealthOrbFill (TextureProgressBar)
│   │   │   │   # 血球填充
│   │   │   │
│   │   │   ├── texture_under: 空球纹理
│   │   │   ├── texture_over: 满球纹理
│   │   │   ├── texture_progress: 渐变球纹理
│   │   │   ├── value: 当前生命值
│   │   │   ├── max_value: 最大生命值
│   │   │   └── fill_mode: FILL_CLOCKWISE
│   │   │
│   │   └── HealthText (Label)
│   │       └── text: "{current}/{max}"
│   │
│   ├── ManaOrbContainer (Control)
│   │   │   # 蓝球容器（对称布局）
│   │   │
│   │   ├── ManaOrbBack (TextureRect)
│   │   ├── ManaOrbFill (TextureProgressBar)
│   │   └── ManaText (Label)
│   │
│   └── ExpBarContainer (Control)
│       │   # 经验条（底部横条）
│       │
│       ├── ExpBarBackground (TextureProgressBar)
│       ├── ExpBarFill (TextureProgressBar)
│       └── LevelLabel (Label)
│           └── text: "Lv.{level}"
│
├── BottomCenter (Control)
│   │   # 底部中央区域
│   │
│   ├── SkillBarContainer (HBoxContainer)
│   │   │   # 技能栏容器
│   │   │
│   │   ├── SkillButton1 (TextureButton)
│   │   │   │   # 技能按钮 1
│   │   │   │
│   │   │   ├── Normal (TextureRect)
│   │   │   │   └── 技能图标
│   │   │   │
│   │   │   ├── Pressed (TextureRect)
│   │   │   │   └── 按下状态
│   │   │   │
│   │   │   ├── Disabled (TextureRect)
│   │   │   │   └── 禁用状态（灰度）
│   │   │   │
│   │   │   ├── CooldownOverlay (ColorRect)
│   │   │   │   │   # CD 覆盖层
│   │   │   │   │
│   │   │   │   └── Color: 半透明黑色
│   │   │   │
│   │   │   ├── ManaCost Label (Label)
│   │   │   │   └── text: "{cost} MP"
│   │   │   │
│   │   │   └── KeyHint (Label)
│   │   │       └── text: "Q" / "W" / "E" / "R"
│   │   │
│   │   ├── SkillButton2 (TextureButton)
│   │   ├── SkillButton3 (TextureButton)
│   │   └── SkillButton4 (TextureButton)
│   │
│   └── PotionButton (TextureButton)
│       │   # 药水按钮
│       │
│       ├── Icon (TextureRect)
│       │   └── texture: 红药水瓶
│       │
│       ├── Quantity Label (Label)
│       │   └── text: "×{quantity}"
│       │
│       └── KeyHint (Label)
│           └── text: "Space"
│
├── TopRight (Control)
│   │   # 右上角区域
│   │
│   ├── MinimapContainer (Panel)
│   │   ├── ViewportContainer (SubViewportContainer)
│   │   │   └── MinimapView (SubViewport)
│   │   │       └── [小地图渲染内容]
│   │   │
│   │   └── MapName Label (Label)
│   │
│   └── QuestTracker (Panel)
│       └── QuestList (VBoxContainer)
│
├── TopLeft (Control)
│   │   # 左上角区域（BUFF 显示）
│   │
│   └── BuffContainer (HBoxContainer)
│       └── BuffIcon1/2/3... (TextureButton)
│
└── CenterScreen (Control)
    │   # 屏幕中央（临时提示）
    │
    ├── DamageNumberContainer (Node)
    │   └── [动态生成的伤害数字]
    │
    ├── LootNotification (Panel)
    │   └── [掉落通知（稀有物品）]
    │
    └── LevelUpNotification (Panel)
        └── [升级通知]
```

### 5.2 背包窗口（InventoryWindow.tscn）

```
InventoryWindow (Window) [脚本：InventoryWindow.gd]
│
├── MarginContainer (MarginContainer)
│   │
│   └── VBoxContainer (VBoxContainer)
│       │
│       ├── TitleBar (HBoxContainer)
│       │   ├── WindowTitle (Label)
│       │   │   └── text: "背包"
│       │   │
│       │   ├── GoldDisplay (Label)
│       │   │   └── text: "💰 {gold}"
│       │   │
│       │   └── CloseButton (Button)
│       │       └── text: "✕"
│       │
│       ├── BagContainer (HBoxContainer)
│       │   │   # 主背包区域
│       │   │
│       │   ├── GridContainer (GridContainer)
│       │   │   │   # 物品格子网格
│       │   │   │
│       │   │   ├── Slot1 (Panel)
│       │   │   │   │   # 物品格子 1
│       │   │   │   │
│       │   │   │   ├── Background (TextureRect)
│       │   │   │   │   └── 格子背景
│       │   │   │   │
│       │   │   │   ├── ItemIcon (TextureRect)
│       │   │   │   │   └── 物品图标（动态加载）
│       │   │   │   │
│       │   │   │   ├── QuantityLabel (Label)
│       │   │   │   │   └── text: "{quantity}" (堆叠物品)
│       │   │   │   │
│       │   │   │   ├── QualityBorder (ColorRect)
│       │   │   │   │   └── 品质边框颜色
│       │   │   │   │
│       │   │   │   └── HighlightOverlay (ColorRect)
│       │   │   │       └── 鼠标悬停高亮
│       │   │   │
│       │   │   ├── Slot2 (Panel)
│       │   │   ├── ... (更多格子)
│       │   │   └── Slot50 (Panel)
│       │   │
│       │   └── ScrollContainer (ScrollContainer)
│       │       └── VScrollBar (VScrollBar)
│       │
│       └── BottomBar (HBoxContainer)
│           │   # 底部工具栏
│           │
│           ├── SortButton (Button)
│           │   └── text: "整理"
│           │
│           ├── FilterOptionButton (Button)
│           │   └── text: "筛选"
│           │
│           └── CapacityLabel (Label)
│               └── text: "{current}/{max} 格"
│
└── DragPreview (Control)
    └── [拖拽预览物品图标]
```

### 5.3 角色窗口（CharacterWindow.tscn）

```
CharacterWindow (Window) [脚本：CharacterWindow.gd]
│
├── MarginContainer (MarginContainer)
│   │
│   └── HBoxContainer (HBoxContainer)
│       │
│       ├── LeftPanel (VBoxContainer)
│       │   │   # 左侧：纸娃娃系统
│       │   │
│       │   ├── PaperDoll (Control)
│       │   │   │   # 角色模型展示
│       │   │   │
│       │   │   ├── CharacterSprite (Sprite2D)
│       │   │   │   └── [2D 角色预览]
│       │   │   │
│       │   │   ├── EquipmentHighlights (Node)
│       │   │   │   ├── HeadHighlight (Marker2D)
│       │   │   │   ├── BodyHighlight (Marker2D)
│       │   │   │   ├── WeaponHighlight (Marker2D)
│       │   │   │   └── AccessoryHighlight (Marker2D)
│       │   │   │
│       │   │   └── EquipmentSlots (Control)
│       │   │       ├── HeadSlot (TextureButton)
│       │   │       ├── BodySlot (TextureButton)
│       │   │       ├── WeaponSlot (TextureButton)
│       │   │       ├── Accessory1Slot (TextureButton)
│       │   │       └── Accessory2Slot (TextureButton)
│       │   │
│       │   └── FlipHorizontalButton (Button)
│       │       └── [左右翻转角色]
│       │
│       └── RightPanel (TabContainer)
│           │   # 右侧：标签页
│           │
│           ├── StatsTab (ScrollContainer)
│           │   │   # 属性标签页
│           │   │
│           │   └── VBoxContainer (VBoxContainer)
│           │       │
│           │       ├── AttributesGroup (VBoxContainer)
│           │       │   │   # 基础属性
│           │       │   │
│           │       │   ├── StrengthContainer (HBoxContainer)
│           │       │   │   ├── Label: "力量"
│           │       │   │   ├── Value: "25"
│           │       │   │   └── PlusButton (Button) [+]
│           │       │   │
│           │       │   ├── AgilityContainer (HBoxContainer)
│           │       │   ├── IntelligenceContainer (HBoxContainer)
│           │       │   └── VitalityContainer (HBoxContainer)
│           │       │
│           │       ├── CombatStatsGroup (VBoxContainer)
│           │       │   │   # 战斗属性
│           │       │   │
│           │       │   ├── DamageContainer (HBoxContainer)
│           │       │   │   ├── Label: "攻击力"
│           │       │   │   └── Value: "150-200"
│           │       │   │
│           │       │   ├── CritRateContainer (HBoxContainer)
│           │       │   │   ├── Label: "暴击率"
│           │       │   │   └── Value: "25%"
│           │       │   │
│           │       │   ├── AttackSpeedContainer (HBoxContainer)
│           │       │   └── ... (更多属性)
│           │       │
│           │       └── ResistancesGroup (VBoxContainer)
│           │           │   # 抗性
│           │           │
│           │           ├── FireResContainer (HBoxContainer)
│           │           ├── ColdResContainer (HBoxContainer)
│           │           ├── LightningResContainer (HBoxContainer)
│           │           └── ... (更多抗性)
│           │
│           ├── SkillsTab (ScrollContainer)
│           │   │   # 技能标签页
│           │   │
│           │   └── SkillTreeContainer (Control)
│           │       └── [2D 技能树可视化]
│           │
│           └── DetailsTab (ScrollContainer)
│               │   # 详细信息标签页
│               │
│               └── DetailedStatsList (VBoxContainer)
│                   └── [高级属性详情]
│
└── AttributePointsDisplay (HBoxContainer)
    ├── Label: "可用属性点："
    └── Value: "5"
```

---

## 六、世界场景结构

### 6.1 城镇场景（Town.tscn）

```
Town (Node2D)
│
├── Environment (Node2D)
│   └── [环境设置]
│
├── Ground (StaticBody2D)
│   ├── Sprite (Sprite2D)
│   │   └── texture: 地面纹理
│   │
│   └── CollisionPolygon2D (CollisionPolygon2D)
│       └── [地面碰撞多边形]
│
├── Buildings (Node2D)
│   ├── BlacksmithHouse (StaticBody2D)
│   │   ├── Sprite (Sprite2D)
│   │   └── CollisionPolygon2D
│   │
│   ├── Tavern (StaticBody2D)
│   └── MagicShop (StaticBody2D)
│
├── NPCs (Node2D)
│   ├── BlacksmithNPC (CharacterBody2D)
│   │   ├── Sprite (Sprite2D)
│   │   ├── DialogueTrigger (Area2D)
│   │   └── ShopData (Resource) [铁匠商店]
│   │
│   ├── PotionVendor (CharacterBody2D)
│   │   └── ShopData (Resource) [药水商]
│   │
│   └── QuestGiver (CharacterBody2D)
│       └── QuestData (Resource) [任务 NPC]
│
├── Waypoint (Marker2D)
│   └── [传送点激活状态]
│
├── NavigationRegion2D (NavigationRegion2D)
│   └── navigation_polygon: NavigationPolygon [烘焙的 2D 导航网格]
│
└── Decoration (Node2D)
    ├── Trees (Node2D)
    │   └── Sprite (Sprite2D)
    ├── Barrels (Node2D)
    └── Crates (Node2D)
```

### 6.2 地下城场景（Dungeon.tscn）

```
Dungeon (Node2D)
│
├── Environment (Node2D)
│   └── [更暗的环境设置]
│
├── DungeonGenerator (Node) [脚本：DungeonGenerator.gd]
│   │   # 2D 随机地图生成器
│   │
│   ├── RoomPrefabs (Array)
│   │   └── [2D 房间预制体列表]
│   │
│   ├── CorridorPrefab (PackedScene)
│   │   └── [2D 走廊预制体]
│   │
│   └── GenerationSettings (Resource)
│       ├── min_rooms: 5
│       ├── max_rooms: 10
│       └── room_size: Vector2i(20, 20)
│
├── SpawnedRooms (Node2D)
│   └── [动态生成的 2D 房间实例]
│
├── EnemySpawnPoints (Node2D)
│   ├── SpawnPoint1 (Marker2D)
│   ├── SpawnPoint2 (Marker2D)
│   └── ... (多个生成点)
│
├── LootChests (Node2D)
│   ├── Chest1 (StaticBody2D)
│   │   ├── Sprite (Sprite2D)
│   │   ├── InteractionArea (Area2D)
│   │   └── LootTable (Resource)
│   │
│   └── Chest2 (StaticBody2D)
│
├── Traps (Node2D)
│   ├── SpikeTrap1 (Area2D)
│   │   ├── CollisionShape2D
│   │   ├── DamageTrigger (Timer)
│   │   └── Visual (Sprite2D)
│   │
│   └── FireTrap1 (Area2D)
│
├── ExitPortal (Area2D)
│   │   # 出口传送门
│   │
│   ├── CollisionShape2D
│   ├── Sprite (Sprite2D)
│   ├── GPUParticles2D (传送特效)
│   └── body_entered 信号连接
│
└── NavigationRegion2D (NavigationRegion2D)
    └── [动态烘焙的 2D 导航网格]
```

---

## 七、资源文件结构

### 7.1 物品数据资源（ItemDatabase.tres）

```gdscript
# ItemDatabase.gd (自定义资源类)
extends Resource
class_name ItemDatabase

@export var items: Array[ItemData]

# ItemData.gd (物品数据类)
extends Resource
class_name ItemData

@export var item_id: String
@export var item_name: String
@export var item_type: String  # "weapon", "armor", "potion", etc.
@export var quality: int  # 0=common, 1=magic, 2=rare, 3=legendary
@export var icon: Texture2D
@export var sprite: SpriteFrames  # 2D 动画帧
@export var base_stats: Dictionary
@export var affixes: Array[String]
@export var required_level: int
@export var sell_price: int
```

### 7.2 技能数据资源

```gdscript
# SkillDatabase.gd
extends Resource
class_name SkillDatabase

@export var skills: Array[SkillData]

# SkillData.gd
extends Resource
class_name SkillData

@export var skill_id: String
@export var skill_name: String
@export var skill_type: String  # "active", "passive"
@export var mana_cost: int
@export var cooldown: float
@export var damage_multiplier: float
@export var effect_description: String
@export var icon: Texture2D
@export var effect_scene: PackedScene  # 2D 特效场景
```

---

## 八、节点层级命名规范

### 8.1 节点命名约定

```gdscript
【前缀规范】
- 2D 节点：不加前缀，使用具体类型名
- 容器节点：xxxContainer
- 分组节点：xxxGroup
- 动态生成：使用复数形式，如 Enemies, Items

【脚本命名】
- 场景脚本：与场景同名，如 Player.gd
- 组件脚本：xxxComponent.gd
- 资源脚本：xxxData.gd 或 xxxResource.gd
- 单例脚本：xxxManager.gd 或 System.gd

【信号命名】
- 动词过去式：damaged, died, picked_up
- 名词 + 动词：item_collected, quest_completed
- 状态变化：health_changed, level_up
```

### 8.2 图层和分组

```gdscript
// 碰撞层定义（Physics Layer 2D）
Layer 1: Static Environment (静态环境)
Layer 2: Enemies (敌人)
Layer 3: Players (玩家)
Layer 4: Projectiles (投射物)
Layer 5: Loot (掉落物)
Layer 6: Traps (陷阱)

// 掩码设置示例
# 玩家射线检测掩码（只检测敌人和环境）
attack_ray.collision_mask = (1 << 1) | (1 << 2)

# 敌人检测掩码（只检测玩家和环境）
enemy_detection.collision_mask = (1 << 1) | (1 << 3)
```

---

## 九、2D 特定功能

### 9.1 2D 光照系统

```
使用 Godot 4 的 2D 光照系统：

1. PointLight2D - 点光源
   - 用于物品发光、技能特效
   - 支持颜色和能量调节

2. DirectionalLight2D - 平行光
   - 模拟全局光照
   - 用于昼夜循环

3. OccluderPolygon2D - 遮光多边形
   - 创建光照阴影
   - 增强场景立体感
```

### 9.2 2D 粒子系统

```
GPUParticles2D 常用效果：

1. 技能特效
   - 魔法飞弹轨迹
   - 爆炸效果
   - 光环效果

2. 环境特效
   - 下雨/下雪
   - 灰尘/雾气
   - 火焰燃烧

3. 反馈特效
   - 受击火花
   - 升级光环
   - 掉落光柱
```

### 9.3 2D 动画系统

```
AnimatedSprite2D 动画管理：

1. SpriteFrames 资源
   - 定义动画帧序列
   - 设置每帧时长
   - 配置循环选项

2. 常用动画
   - Idle: 待机呼吸动画
   - Run/Walk: 移动动画
   - Attack: 攻击动作
   - Hit: 受击反应
   - Death: 死亡动画

3. 动画混合
   - 使用 AnimationTree
   - 实现平滑过渡
   - 状态机控制
```

---

## 十、场景切换流程

```gdscript
# 2D 场景加载流程图

当前场景 (Current Scene)
    ↓
[保存当前状态]
    ↓
[卸载当前场景]
    ↓
[显示加载界面]
    ↓
[异步加载新场景]
    ↓
[初始化新场景数据]
    ↓
[隐藏加载界面]
    ↓
新场景 (New Scene)
```

---

*文档版本：v1.0 (2D 版)*  
*创建日期：2026-04-02*  
*适用引擎：Godot 4.x*  
*游戏类型：2D 俯视角 ARPG*
