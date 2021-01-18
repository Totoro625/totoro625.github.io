---
title: 我的世界worldedit常用命令
date: 2017-12-19 22:43:00
tags: 
  - minecraft
  - worldedit
category: bash
---
*从入门到跑路*

一定要好好学习指令，免得把世界弄崩溃

`//undo`和`//redo`要抓好，指令输错可挽回

## 我最喜欢用的是

| 命令               | 描述         |
| ---------------- | ---------- |
| //set id         | 改变选区方块     |
| //replace        | 替换选区方块     |
| //walls          | 选区设置围墙     |
| //stack          | 复制选区       |
| /repl            | 把右键变成小刷子   |
| /brush clipboard | 绑定一个工具为大刷子 |
| /jumpto          | 传送         |
| //copy           | 复制         |
| //paste          | 粘贴         |
| //save           | 保存剪贴板      |
| //load           | 导入剪贴板      |

<!-- more -->

## 常用命令

*至少学会这些才能开始愉快的玩耍*

| **命令**           | **参数**                  | **描述**                                   |
| ---------------- | ----------------------- | ---------------------------------------- |
| //wand           |                         | 给你一个设置选区工具（默认为木斧）。单击左键设置点1；右键设置点2。       |
| //set id         |                         | 将选中的区域变为某物品(如果是不可放置或者没有放置点的物品会成为被扔出的状态)  |
| //undo           |                         | 撤销上一次的WorldEdit行动                        |
| //redo           |                         | 还原上一次的//undo                             |
| //move x         |                         | 将区域移动x块。移动时需正对着你要移动的区域，他就会帮你移动到你正对的地方    |
| //move x up      |                         | 将区域向上移动x块                                |
| //move x down    |                         | 将区域向下移动x块                                |
| //limit          | <限制值>                   | 设置最大操作方块数量，只对你生效。这是为了防止操作失误引起的灾难性事故，它不会将配置参数覆盖。 |
| /clearhistory    |                         | 清理所有历史记录                                 |
| //replace        | <to-block>              | 替换选区内所有方块（不包含空气）                         |
| //replace        | <from-block> <to-block> | 将选区内的某一种方块替换为另外一种方块                      |
| //walls          | <block>                 | 填充选区边界（仅包含Z轴）                            |
| //outline        | <block>                 | 填充选区边界（常规的建筑体，内部空心）                      |
| /jumpto          |                         | 传送到指向的方块上                                |
| //stack          |                         | 复制选区x份，向指向方向                             |
| //copy           |                         | 复制选区                                     |
| //cut            |                         | 剪切选区                                     |
| //paste          | [-ao]                   | 粘贴选区                                     |
| //rotate         | <angle-in-degrees>      | 旋转剪切板内容                                  |
| //flip           | [dir]                   | 翻转剪切板内容                                  |
| //load           | <filename>              | 读取一个剪切板文件内容                              |
| //save           | <filename>              | 将剪切板内容保存为文件（可以尝试选择一座建筑保存为单独的文件，然后使用时读取）  |
| /clearclipboard  |                         | 清除剪切板                                    |
| //regen          |                         | 重置选区                                     |
| /repl            | <block>                 | 工具右键替换为某个方块                              |
| /brush clipboard |                         | 设定刷子形状为剪贴板内容                             |

## 快速生成
### 生成指令
| **命令**     | **参数**                     | **描述**     |
| ---------- | -------------------------- | ---------- |
| //hcyl     | <block> <radius> [height]  | 创建一个垂直空心圆柱 |
| //cyl      | <block> <radius> [height]  | 创建一个垂直圆柱   |
| //sphere   | <block> <radius> [raised?] | 创建一个球体     |
| //hsphere  | <block> <radius> [raised?] | 创建一个空心球体   |
| /forestgen | [size] [type] [density]    | 创建一片森林     |
| /pumpkins  | [size]                     | 创建一片南瓜森林   |
| //fill     | <block> <radius> [depth]   | 圆柱         |
| //fillr    | <block> <radius>           | 半圆球        |
### 模板刷子
| **命令**           | **参数**                        | **描述**                                   |
| ---------------- | ----------------------------- | ---------------------------------------- |
| /none            |                               | 切换无工具                                    |
| /info            |                               | 显示工具信息                                   |
| /tree            | [type]                        | 工具右键创建树或其他， [tree, regular, big, bigtree, redwood, sequoia, tallredwood, tallsequoia, birch, white, whitebark, pine, randredwood, randomredwood, anyredwood, rand, random] |
| /repl            | <block>                       | 工具右键替换为某个方块                              |
| /cycler          |                               | 切换方块的其他类型，如各种原木、树叶、泥土                    |
| /brush sphere    | [-h] <type> <radius>          | 设定刷子形状为球体                                |
| /brush cylinder  | [-h] <type> <radius> [height] | 设定刷子形状为圆柱                                |
| /brush clipboard |                               | 设定刷子形状为剪贴板内容                             |
| /brush smooth    | <radius> [iterations]         | 设定刷子形状为光滑的平面                             |
| /size            | <size>                        | 设置笔刷大小                                   |
| /mat             | <mat>                         | 改变当前刷子使用的材料                              |
| /mask            |                               | 清除遮罩                                     |
| /mask            | <mask>                        | 设置一个遮罩                                   |
### 生成的太快人得传送出来
| **命令**   | **参数**      | **描述**    |
| -------- | ----------- | --------- |
| /unstuck |             | 去一个空旷的地方  |
| /ascend  |             | 向上传送一层    |
| /descend |             | 向下传送一层    |
| /ceil    | [clearance] | 传送到顶部     |
| /thru    |             | 穿过障碍物     |
| /jumpto  |             | 传送到指向的方块上 |
| /up      | [distance]  | 向上传送      |

​          

## 部分指令翻译

*不完整*

来源：[我的世界游戏园](http://minecraft.yxzoo.com/38477)

| **命令**             | **参数**                                | **描述**                                   |
| ------------------ | ------------------------------------- | ---------------------------------------- |
| //limit            | <限制值>                                 | 设置最大操作方块数量，只对你生效。这是为了防止操作失误引起的灾难性事故，它不会将配置参数覆盖。 |
| **历史**             |                                       |                                          |
| //undo             | [步骤值]                                 | 撤销上一次操作                                  |
| //redo             | [步骤值]                                 | 还原操作，仅还原上一次操作，不能重复上一次命令                  |
| /clearhistory      |                                       | 清理所有历史记录                                 |
| **选择**             |                                       |                                          |
| //wand             |                                       | 给你一个设置选区工具（默认为木斧）。单击左键设置点1；右键设置点2。       |
| /toggleeditwand    |                                       | 设置编辑选区工具的模式，可以让工具恢复正常。                   |
| //sel              | <cuboid\poly>                        | 选择选区的形状                                  |
| //pos1             |                                       | 将玩家脚下的方块设置为点1                            |
| //pos2             |                                       | 将玩家脚下的方块设置为点2                            |
| //hpos1            |                                       | 将玩家指向的方块设置为点1                            |
| //hpos2            |                                       | 将玩家指向的方块设置为点2                            |
| //chunk            |                                       | 将选区修改为你附近的16*16*Max大小的区域（PS：MAX指Z轴最大化）   |
| //expand           | <amount>                              | 根据玩家朝向扩大选区                               |
| //expand           | <amount> <direction>                  | 根据参数设定方向扩大（north, east, south, west, up, down） |
| //expand           | <amount> <reverse-amount> [direction] | 将选区朝两个方向同时扩大                             |
| //expand           | vert                                  | 扩大选区（包含地壳）                               |
| //contract         | <amount>                              | 根据玩家朝向缩小选区                               |
| //contract         | <amount> [direction]                  | 根据参数设定方向缩小选区（north, east, south, west）   |
| //contract         | <amount> <reverse-amount> [direction] | 将选区朝两个方向同时缩小                             |
| //outset           | [-hv] <amount>                        | 按照参数整体放大选区                               |
| //inset            | [-hv] <amount>                        | 按照参数整体缩小选区                               |
| //shift            | <amount> [direction]                  | 移动选区，但不移动选区内的方块                          |
| //size             |                                       | 获得选区内的方块数量                               |
| //count            | <block>                               | 获取选区内某一种方块的数量                            |
| //distr            | [-c]                                  | 获取选区内方块的分布情况                             |
| **选区操作**           |                                       |                                          |
| //set              | <block>                               | 修改区域内所有方块                                |
| //replace          | <to-block>                            | 替换选区内所有方块（不包含空气）                         |
| //replace          | <from-block> <to-block>               | 将选区内的某一种方块替换为另外一种方块                      |
| //overlay          | <block>                               | 在选区内的方块上放置方块                             |
| //walls            | <block>                               | 填充选区边界（仅包含Z轴）                            |
| //outline          | <block>                               | 填充选区边界（常规的建筑体，内部空心）                      |
| //smooth           | [iterations]                          | 向下压缩选区                                   |
| //regen            |                                       | 重置选区                                     |
| //move             | [count] [direction] [leave-id]        | 移动选区                                     |
| //stack            | [count] [direction]                   | 叠加选择                                     |
| **剪切板**            |                                       |                                          |
| //copy             |                                       | 复制选区                                     |
| //cut              |                                       | 剪切选区                                     |
| //paste            | [-ao]                                 | 粘贴选区                                     |
| //rotate           | <angle-in-degrees>                    | 旋转剪切板内容                                  |
| //flip             | [dir]                                 | 翻转剪切板内容                                  |
| //load             | <filename>                            | 读取一个剪切板文件内容                              |
| //save             | <filename>                            | 将剪切板内容保存为文件（可以尝试选择一座建筑保存为单独的文件，然后使用时读取）  |
| /clearclipboard    |                                       | 清楚剪切板                                    |
| **快速创建**           |                                       |                                          |
| //hcyl             | <block> <radius> [height]             | 创建一个垂直空心圆柱                               |
| //cyl              | <block> <radius> [height]             | 创建一个垂直圆柱                                 |
| //sphere           | <block> <radius> [raised?]            | 创建一个球体                                   |
| //hsphere          | <block> <radius> [raised?]            | 创建一个空心球体                                 |
| /forestgen         | [size] [type] [density]               | 创建一片森林                                   |
| /pumpkins          | [size]                                | 创建一片南瓜森林                                 |
| **公共**             |                                       |                                          |
| /toggleplace       |                                       | 设置两点（不太明白，求补充……）Toggle between using pos #1 or your current position. |
| //fill             | <block> <radius> [depth]              | Fill a hole.                             |
| //fillr            | <block> <radius>                      | Fill a hole fully recursively.           |
| //drain            | <radius>                              | 删除周围的水和岩浆                                |
| /fixwater          | <radius>                              | 修正玩家附近水池内的水流为水源                          |
| /fixlava           | <radius>                              | 修正玩家附近岩浆池内的流动岩浆为岩浆源                      |
| /removeabove       | [size] [height]                       | 删除玩家头顶的方块                                |
| /removebelow       | [size] [height]                       | 删除玩家脚下的方块                                |
| /replacenear       | <size> <from-id> <to-id>              | 替换玩家周围的某种方块                              |
| /removenear        | [block] [size]                        | 删除玩家周围的方块                                |
| /snow              | [radius]                              | 在玩家周围创建积雪                                |
| /thaw              | [radius]                              | 删除玩家周围的积雪                                |
| /ex                | [size]                                | 扑灭玩家周围的火灾                                |
| /butcher           | [radius]                              | 杀死玩家周围的怪物                                |
| /remove            | <type> <radius>                       | 删除玩家周围一切实体                               |
| **块工具**            |                                       |                                          |
| /chunkinfo         |                                       | 获取玩家所处块的文件名                              |
| /listchunks        |                                       | 显示所有块列表                                  |
| /delchunks         |                                       | 使用一个shell脚本删除块                           |


| **命令**             | **参数**                                | **描述**                                   |
| ------------------ | ------------------------------------- | ---------------------------------------- |
| **超级工具**           |                                       |                                          |
| //                 |                                       | 设置超级工具                                   |
| /sp single         |                                       | 切换到单块超级工具                                |
| /sp area           | <range>                               | 切换到大面积超级工具                               |
| /sp recur          | <Range>                               | 切换到超级递归工具                                |
| **工具**             |                                       |                                          |
| /none              |                                       | 切换无工具                                    |
| /info              |                                       | 显示工具信息                                   |
| /tree              | [type]                                | 工具右键创建树或其他， [tree, regular, big, bigtree, redwood, sequoia, tallredwood, tallsequoia, birch, white, whitebark, pine, randredwood, randomredwood, anyredwood, rand, random] |
| /repl              | <block>                               | 工具右键替换为某个方块                              |
| /cycler            |                                       | Block data cycler tool.                  |
| **刷子**             |                                       |                                          |
| /brush sphere      | [-h] <type> <radius>                  | 设定刷子形状为球体                                |
| /brush cylinder    | [-h] <type> <radius> [height]         | 设定刷子形状为圆柱                                |
| /brush clipboard   |                                       | 设定刷子形状为剪贴板内容                             |
| /brush smooth      | <radius> [iterations]                 | 设定刷子形状为光滑的平面                             |
| /size              | <size>                                | 设置笔刷大小                                   |
| /mat               | <mat>                                 | 改变当前刷子使用的材料                              |
| /mask              |                                       | 清除遮罩                                     |
| /mask              | <mask>                                | 设置一个遮罩                                   |
| **传送**             |                                       |                                          |
| /unstuck           |                                       | 去一个空旷的地方                                 |
| /ascend            |                                       | 向上传送一层                                   |
| /descend           |                                       | 向下传送一层                                   |
| /ceil              | [clearance]                           | 传送到顶部                                    |
| /thru              |                                       | 穿过障碍物                                    |
| /jumpto            |                                       | 传送到指向的方块上                                |
| /up                | [distance]                            | 向上传送                                     |
| **花絮**             |                                       |                                          |
| //restore          | [snapshot]                            | 还原到指定的快照                                 |
| /snap use          | <snapshot>                            | 使用特定的快照                                  |
| /snap list         | [num]                                 | 显示快照列表                                   |
| /snap before       | <date>                                | 回到上一个快照                                  |
| /snap after        | <date>                                | 进入下一个快照                                  |
| **脚本**             |                                       |                                          |
| /cs                |                  | 执行一个脚本                                   |
| /.s                | [args]                             | 重新执行最后一个脚本                               |
| /xxx.js       | [args]                             | 执行一个JS脚本                                 |
| **通用命令**           |                                       |                                          |
| /search            | <item>                                | 搜索物品名称                                   |
| /worldedit reload  |                                       | 重置WorldEdit配置                            |
| /worldedit version |                                       | 显示WorldEdit版本                            |
| /worldedit tz      |                                       | 设置时区（暂时）                                 |

  

## 完整英文指令

相对完整，来自[wiki参考版](http://wiki.sk89q.com/wiki/WorldEdit/Reference)。同时可以查看[wiki权威版](http://wiki.sk89q.com/wiki/WorldEdit/Permissions) 获取更加精确的描述

| Command                                  | Parameters                               | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| //limit                                  | <limit>                                  | Set a maximum number of blocks to change at most for all operations. This only affects you. Use this to prevent catastrophic accidents. This command will not override the limit in the [configuration](http://wiki.sk89q.com/wiki/WorldEdit/Configuration) if it is set. |
| [History](http://wiki.sk89q.com/wiki/WorldEdit/Features#History) |                                          |                                          |
| //undo                                   | [num-steps] [player]                     | Undo your last action.                   |
| //redo                                   | [num-steps] [player]                     | Redo your last (undone) action. This command replays back history and does not repeat the command. |
| /clearhistory                            |                                          | Clear your history.                      |
| [Selection](http://wiki.sk89q.com/wiki/WorldEdit/Selection) |                                          |                                          |
| //wand                                   |                                          | Gives you the "edit wand" (by default, a wooden axe). Left click with this tool to select position 1 and right click to selection position 2. |
| /toggleeditwand                          |                                          | Toggles the edit wand selection mode, allowing you to use the edit wand item normally. |
| //sel                                    | [-d]  | Choose the region shape to use for selections. <cuboid\extend\poly\ellipsoid\sphere\cyl\convex>|
| //desel                                  |                                          | Deselects the current selection.         |
| //pos1                                   | [x,y,z]                                  | Set selection position #1 to the block above the one that you are standing on. |
| //pos2                                   | [x,y,z]                                  | Set selection position #2 to the block above the one that you are standing on. |
| //hpos1                                  |                                          | Set selection position #1 to the block that you are looking at. |
| //hpos2                                  |                                          | Set selection position #2 to the block that you are looking at. |
| //chunk                                  | [-sc] [x,z]                              | Select the chunk that you are within for your selection. |
| //expand                                 | <amount>                                 | Expands the selection in the direction that you are looking. |
| //expand                                 | <amount> <direction>                     | Expands the selection in the specified direction (north, east, south, west, up, down). |
| //expand                                 | <amount> <reverse-amount> [direction]    | Expands the selection in two directions at once. |
| //expand                                 | vert                                     | Expands the selection to include sky to bedrock. |
| //contract                               | <amount>                                 | Contracts the selection in the direction that you are looking. |
| //contract                               | <amount> [direction]                     | Contracts the selection in the specified direction (north, east, south, west, up, down). |
| //contract                               | <amount> <reverse-amount> [direction]    | Contracts the selection in two directions at once. |
| //outset                                 | [-hv] <amount>                           | Outset the selection in every direction. |
| //inset                                  | [-hv] <amount>                           | Inset the selection in every direction.  |
| //shift                                  | <amount> [direction]                     | Moves the selection region. It does not move the selection's contents. |
| //size                                   | [-c]                                     | Get the size of selected region.         |
| //count                                  | [-d] <block>                             | Count the number of blocks in the region. |
| //distr                                  | [-cd]                                    | Get the block distribution in the selection. |
| [Region operations](http://wiki.sk89q.com/wiki/WorldEdit/Region_operations) |                                          |                                          |
| //set                                    | <block>                                  | Set all blocks inside the selection region to a specified block. |
| //replace                                | <to-block>                               | Replace all non-air blocks inside the region. |
| //replace                                | <from-block> <to-block>                  | Replace all blocks of the specified block(s) with another block inside the region. |
| //overlay                                | <block>                                  | Place a block on top of blocks inside the region. |
| //walls                                  | <block>                                  | Build the walls of the region (not including ceiling and floor). |
| //outline                                | <block>                                  | Build walls, floor, and ceiling.         |
| //center                                 | <block>                                  | Set the center block(s).                 |
| //smooth                                 | [-n] [iterations]                        | Smooth the selection's heightmap.        |
| //deform                                 | [-ro] <expression>                       | Deforms a selected region with an expression. |
| //hollow                                 | [thickness] [block]                      | Hollows out objects contained in the selection. |
| //regen                                  |                                          | Regenerate the selection region.         |
| //move                                   | [-s] [count] [direction] [leave-id]      | Move the selection's contents. A block can be specified to fill in the left over area. |
| //stack                                  | [-sa] [count] [direction]                | Stacks the selection.                    |
| //naturalize                             |                                          | Sets 3 layers of dirt on top of the surface of the selection, and then rock below. |
| //line                                   | [-h] <block> [thickness]                 | Draws a line segment between cuboid selection corners. |
| //curve                                  | [-h] <block> [thickness]                 | Draws a spline between cuboid selection corners. |
| //forest                                 | [type] [density]                         | Make a forest within the region.         |
| //flora                                  | [density]                                | Make flora within the region.            |
| [Clipboard](http://wiki.sk89q.com/wiki/WorldEdit/Clipboard) |                                          |                                          |
| //copy                                   | [-m]                                     | Copies the currently selected region. Be aware that it stores your position relative to the selection when copying. |
| //cut                                    | [-m]                                     | Cuts the currently selected region.      |
| //paste                                  | [-sao] [-a]                              | Pastes the clipboard. If you paste with -a, air blocks are being ignored. |
| //rotate                                 | <y-axis> [x-axis] [z-axis]               | Rotate the clipboard.                    |
| //flip                                   | [direction]                              | Flip the clipboard.                      |
| //schematic or //schem                   | save [<format>] <filename>               | Save clipboard to .schematic. (mcedit is currently the only format) |
| //schematic or //schem                   | delete <filename>                        | Delete a saved schematic.                |
| //schematic or //schem                   | load [<format>] <filename>               | Load .schematic to clipboard.            |
| //schematic or //schem                   | list [-dn] [-p <page>]                   | List all available schematics.           |
| //schematic or //schem                   | formats                                  | Display available schematic formats.     |
| /clearclipboard                          |                                          | Clear your clipboard.                    |
| [Generation](http://wiki.sk89q.com/wiki/WorldEdit/Generation) |                                          |                                          |
| //generate                               | [-hroc] <block> <expression>             | Generates a shape according to a given formula. |
| //generatebiome                          | [-hroc] <block> <expression>             | Sets biome according to a given formula. |
| //hcyl                                   | <block> <radius>[,<radius>] [height]     | Create a vertical hollow cylinder.       |
| //cyl                                    | [-h] <block> <radius>[,<radius>] [height] | Create a vertical cylinder.              |
| //sphere                                 | [-h] <block> <radius>[,<radius>,<radius>]  | Create a sphere.   [raised? true\false (default)]                      |
| //hsphere                                | <block> <radius>[,<radius>,<radius>]  | Create a hollow sphere.  [raised? true\false (default)]                |
| //pyramid                                | [-h] <block> <size>                      | Create a pyramid.                        |
| //hpyramid                               | <block> <size>                           | Create a hollow pyramid.                 |
| /forestgen                               | [size] [type] [density]                  | Make a forest.                           |
| /pumpkins                                | [size]                                   | Make a pumpkin forest                    |
| [Utilities](http://wiki.sk89q.com/wiki/WorldEdit/Utilities) |                                          |                                          |
| /toggleplace                             |                                          | Toggle between using pos #1 or your current position. |
| //fill                                   | <block> <radius> [depth]                 | Fill a hole.                             |
| //fillr                                  | <block> <radius> [depth]                 | Fill a hole fully recursively.           |
| //drain                                  | <radius>                                 | Drain nearby water/lava pools.           |
| /fixwater                                | <radius>                                 | Level nearby pools of water.             |
| /fixlava                                 | <radius>                                 | Level nearby pools of lava.              |
| /removeabove                             | [size] [height]                          | Remove blocks above your head.           |
| /removebelow                             | [size] [height]                          | Remove blocks below your feet.           |
| /replacenear                             | [-f] <size> <from-id> <to-id>            | Replace all existing blocks nearby.      |
| /removenear                              | <block> [size]                           | Remove blocks near you.                  |
| /snow                                    | [radius]                                 | Simulate snow cover.                     |
| /thaw                                    | [radius]                                 | Thaw/remove snow.                        |
| /ex                                      | [radius]                                 | Extinguish fires.                        |
| /butcher                                 | [-plangbtfr] [radius]                    | Kill nearby mobs.                        |
| /remove                                  | <type> <radius>                          | Remove nearby entities. Type can be "items" for loose items/drops, arrows, boats, minecarts, tnt, or xp. |
| /green                                   | [-f] [radius]                            | Greens the area.                         |
| //calc                                   | <expression>                             | Evaluate a mathematical expression.      |
| [Chunk tools](http://wiki.sk89q.com/wiki/WorldEdit/Chunk_tools) |                                          |                                          |
| /chunkinfo                               |                                          | Get the filename of the chunk that you are in. |
| /listchunks                              |                                          | Print a list of used chunks.             |
| /delchunks                               |                                          | Generate a shell script to delete chunks. |



| Command                                  | Parameters                               | Description                              |
| ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| [Super pickaxe](http://wiki.sk89q.com/wiki/WorldEdit/Super_pickaxe) |                                          |                                          |
| //                                       |                                          | Toggle the super pick axe.               |
| /sp single                               |                                          | Switch to single block super pickaxe mode. |
| /sp area                                 | <range>                                  | Switch to area super pickaxe mode.       |
| /sp recur                                | <range>                                  | Switch to recursive super pickaxe mode.  |
| [Tools](http://wiki.sk89q.com/wiki/WorldEdit/Tools) |                                          |                                          |
| /tool                                    |  										| Select a tool to bind. <repl\cycler\floodfill\brush\lrbuild\tree\deltree\farward\info\none>                  |
| /none                                    |                                          | Switch to no tool.                       |
| /info                                    |                                          | Switch to the info tool.                 |
| /farwand                                 |                                          | Wand at a distance tool.                 |
| /lrbuild                                 | <leftclick block> <rightclick block>     | Long-range building tool.                |
| /tree                                    | [type]                                   | Switch to the tree tool. Available types [tree, regular, big, bigtree, redwood, sequoia, tallredwood, tallsequoia, birch, white, whitebark, pine, randredwood, randomredwood, anyredwood, rand, random] |
| /deltree                                 |                                          | Floating tree remover tool.              |
| /repl                                    | <block>                                  | Switch to the block replacer tool.       |
| /cycler                                  |                                          | Block data cycler tool.                  |
| /flood                                   | <pattern> <range>                        | Flood fill tool.                         |
| [Brushes](http://wiki.sk89q.com/wiki/WorldEdit/Brushes) |                                          |                                          |
| /brush sphere                            | [-h] <type> [radius]                     | Switch to the sphere brush tool.         |
| /brush cylinder                          | [-h] <type> [radius] [height]            | Switch to the cylinder brush tool.       |
| /brush clipboard                         | [-ap]                                    | Switch to the clipboard tool.            |
| /brush smooth                            | [-n] [radius] [iterations]               | Smooth the region.                       |
| /brush extinguish                        | [radius]                                 | Switch to the fire extinguisher brush tool. |
| /brush gravity                           | [-h] [radius]                            | Switch to the gravity simulator brush tool. |
| /brush butcher                           | [-plangbtfr] [radius]                    | Switch to the entity butcher brush tool. |
| /size                                    | <size>                                   | Change the size of the current brushes   |
| /mat                                     | <pattern>                                | Change the material used by your current brush. |
| /range                                   | <range>                                  | Set the brush range.                     |
| /mask                                    |                                          | Clear the mask                           |
| /mask                                    | <mask>                                   | Set a mask                               |
| //gmask                                  | <mask>                                   | Set the global mask.                     |
| [Getting around](http://wiki.sk89q.com/wiki/WorldEdit/Getting_around) |                                          |                                          |
| /unstuck                                 |                                          | Go up to the first free spot.            |
| /ascend                                  | [num-levels]                             | Go up levels.                            |
| /descend                                 | [num-levels]                             | Go down levels.                          |
| /ceil                                    | [clearance]                              | Get to the ceiling.                      |
| /thru                                    |                                          | Go through the wall that you are looking at. |
| /jumpto                                  |                                          | Jump to the block that you are looking at. |
| /up                                      | [-fg] [distance]                         | Go up some distance.                     |
| [Snapshots](http://wiki.sk89q.com/wiki/WorldEdit/Snapshots) |                                          |                                          |
| //restore                                | [snapshot]                               | Restore a particular snapshot.           |
| //snapshot use                           | <snapshot>                               | Use a particular snapshot.               |
| //snapshot sel                           | <index>                                  | Choose the snapshot based on the list id. |
| //snapshot list                          | [num]                                    | List the 5 newest snapshots.             |
| //snapshot before                        | <date>                                   | Find the first snapshot before the given date. |
| //snapshot after                         | <date>                                   | Find the first snapshot after the given date. |
| [Scripting](http://wiki.sk89q.com/wiki/WorldEdit/Scripting) |                                          |                                          |
| /cs                                      |                       | Executes a script.                       |
| /.s                                      | [args]                                | Re-executes last script with new arguments. |
| /xxx.js                             | [args]                                | Executes a JS script.                    |
| [General commands](http://wiki.sk89q.com/wiki/WorldEdit/General_commands) |                                          |                                          |
| /searchitem                              |                                          | Search for an item by its name.          |
| /worldedit                               |                                          | WorldEdit commands.                      |
| /worldedit help                          | [command]                                | Displays help documentation for the given command, or lists all commands if none are given (aliased to //help). |
| /worldedit reload                        |                                          | Reloads WorldEdit's configuration.       |
| /worldedit version                       |                                          | Gets WorldEdit's version.                |
| /worldedit tz                            |                                          | Set your time zone. This is temporary.   |
| //fast                                   |                                          | Toggles fastmode performance.            |
| Biomes                                   |                                          |                                          |
| /biomelist                               | [page]                                   | List the available biome types           |
| /biomeinfo                               | [-pt]                                    | Get the biome of the targeted block.     |
| //setbiome                               | [-p] <biome type>                        | Set the selected area to the specified biome type.The -p flag changes the block (column) you are standing in. |