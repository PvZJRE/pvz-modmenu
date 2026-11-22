# ModMenu — 模组管理界面

游戏内的模组管理界面：主菜单 → 游戏选项 → "模 组"按钮（原"查看图鉴"槽位，由 Mixin 注入，仅主菜单变体显示）打开。左侧为模组列表，右侧为选中模组的详情面板（链接按钮、许可证、作者/贡献者）。

## 界面

- **左侧**：模组列表——44×44 图标框 + 模组名/描述，选中高亮，滚动条翻页；
- **右侧**：详情面板——名称、版本（id）、描述、三枚原生石钮（官网 / 问题 / 配置）、命名链接（`sources` → "源码"，`modmenu.<平台>` 键 → 平台名：GitHub/Gitee/QQ群/Discord/哔哩哔哩 等，未收录的键首字母大写回落）、许可证、模组贡献者（作者/贡献者两行）；
- 未设置的字段与按钮不显示或置灰（官网/问题未设置置灰、配置未声明隐藏、链接未设置不画）；
- 点击链接弹出确认框（URL 展示 + "复制链接到剪贴板"），"是"经系统浏览器打开。

## 元数据来源

直接读 Fabric Loader 的 `ModMetadata`（名称/版本/描述/作者/贡献者/许可证）；联系方式按键 `website` / `issues` / `sources` 取，`modmenu.*` 键读为命名链接；图标从 `getIconPath(0)` + `ModContainer.findPath` 读包内 PNG。

列表条目口径与主界面左下角"模组已载入 N 个"一致：游戏本体与 loader 自身计入，排除 builtin 环境占位与 jar-in-jar 嵌套子模块（api-* 子模块、loader 内嵌的 MixinExtras）。

## 模组接入"配置"按钮

在模组的 `fabric.mod.json` 声明入口点，并实现本模组的接口：

```json
"entrypoints": { "modmenu": ["cn.xxx.你的类"] }
```

```java
public final class YourModMenu implements io.github.pvzjre.modmenu.api.ModMenuApi {
    @Override
    public void openConfig() {
        // 叠加打开你自己的模态对话框
    }
}
```

声明后详情面板出现"配 置"按钮。

## 构建 / 安装

```bash
cd ../../ && ./gradlew distZip && cd examples/pvz-modmenu
./gradlew build        # 产物 build/libs/modmenu-1.0.0.jar
```

jar 放进版本目录 `mods/`（需同放 `fabric-api-<版本>.jar`）。

## 许可证

MIT。
