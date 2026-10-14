# 会场 Tab 工作室

静态 HTML，入口为 index.html，无需安装依赖或构建。代码、SVG 路径、字体和 ZIP 库均内嵌，单独打开 index.html 也可以使用。

## 上传

解压后，把 index.html、assets 文件夹、references 文件夹、.nojekyll 和本说明一起上传到 GitHub 仓库根目录。
请上传解压后的文件，不是只上传 ZIP。保留文件夹结构和字体文件名。
需要在线访问时，启用仓库的 GitHub Pages，发布目录选择仓库根目录。

## 内容

- assets/fonts/FZLTZHJW.TTF：方正兰亭中黑简体，Tab 统一 30px，不额外合成加粗。
- assets/svg：中式原始 SVG；assets/svg/regular：新增常规原始 SVG。
- assets/references：36 张历史 PNG 参考图，仅供对照，不参与 Tab 绘制。
- references：原始界面配色与组件配色规范。
- Tab 使用 SVG 原始路径和 Canvas Path2D 绘制，不再使用 PNG 轮廓蒙版。
- 右侧 Tab 样式可选择中式或常规，每组独立设置，颜色沿用同一套深浅配色。
- 颜色在 index.html 的 VECTOR_PALETTES 中独立配置，右侧仅提供会场的深浅色系选择。
- 预览固定为第一个 Tab 选中，其余未选中，点击不会切换状态。
- ZIP 导出逻辑和 SVG 路径已内置于 index.html，不依赖 CDN。

一行二：336 × 88；一行三：226 × 88；一行四（完整显示）：165 × 88；一行 N（可横滑）：180 × 88。间距统一 12px。
上述为声明尺寸；画布和 PNG 始终 @2x，实际像素分别为 672 × 176、452 × 176、330 × 176、360 × 176。12px 间距对应 24 个画布像素。
使用 Canvas API 绘制，toDataURL 导出 PNG；不使用 html2canvas。每个标签导出选中、未选中两种透明 PNG，打包为 ZIP。
编辑数据保存于当前浏览器，不会自动同步到其他设备。

## SG 消息协议

通过 iframe.contentWindow.postMessage 发送对象；回应发回请求窗口并保留 requestId，带 response: true、ok: true/false。响应消息不会被再次处理。
SG_CONFIG 示例：{type:"SG_CONFIG",requestId:"config-1",config:{style:"regular",family:"red",mode:"light",count:4,variant:"fixed",texts:["热门权益","甜点饮品","能量西餐","精选好酒"]}}。
style 为 chinese/regular（也支持中式/常规）；family 为 red/pink/yellow/orange/green/teal/cyan/blue/purple；mode 为 light/dark。dark 布尔值单独控制编辑器深浅色。
可提供 tabs 数组批量替换，每项包含 style、count、variant、texts；selected 为当前组的零基索引。view 为 venue/deep。showBackground 为布尔值。仅更新提供的字段；非法配置返回错误且不修改画面。
SG_EXPORT 示例：{type:"SG_EXPORT",requestId:"export-1",config:{scope:"row",tabIndex:0}}。默认导出当前组的一整行透明 PNG（第一项选中）；N 个会全部导出，不截取横滑窗口。
单个标签：{type:"SG_EXPORT",requestId:"export-2",config:{scope:"asset",tabIndex:0,index:1,selected:false}}。index 为零基标签索引，selected 可指定选中/未选中；省略时仅 index 0 为选中。
成功返回 dataURL、实际 width/height、declaredWidth/declaredHeight、scale:2、scope、style。config 也可写成 payload。失败返回 error。
响应目标 origin 与请求 origin 相同；本地文件 origin 为 null 时使用 *。仅应嵌入受信任的宿主页面，配置和导出不应包含敏感资料。
字体为用户提供，公开发布前请自行确认字体的网络嵌入及分发授权。
