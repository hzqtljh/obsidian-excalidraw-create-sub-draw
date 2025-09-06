// 在旧文件画布 (0,0) 插入 ⮩（指向将创建的新文件），创建并打开新文件并切到 Excalidraw；
// 新文件画布中心放置 ⮪，其链接指回旧文件。
(async () => {
  if (!ea || !ea.setView) { new Notice("未检测到 Excalidraw Automate 环境"); return; }

  async function uniqueBase(dir, base) {
    const ad = app.vault.adapter;
    let name = base, i = 1;
    const pathOf = n => (dir ? `${dir}/` : "") + `${n}.excalidraw.md`;
    while (await ad.exists(pathOf(name))) name = `${base}-${i++}`;
    return name;
  }
  function timestampName() {
    const d = new Date(), p = n => String(n).padStart(2,"0");
    return `${d.getFullYear()}${p(d.getMonth()+1)}${p(d.getDate())}${p(d.getHours())}${p(d.getMinutes())}${p(d.getSeconds())}`;
  }

  function buildFromTemplate(oldTarget) {
    return `---
excalidraw-plugin: parsed
tags: [excalidraw]

---
==⚠  Switch to EXCALIDRAW VIEW in the MORE OPTIONS menu of this document. ⚠== You can decompress Drawing data with the command palette: 'Decompress current Excalidraw file'. For more info check in plugin settings under 'Saving'


# Excalidraw Data

## Text Elements
⮪ ^CXGAUbYb

## Element Links
CXGAUbYb: [](${oldTarget})

%%
## Drawing
\`\`\`json
{
  "type": "excalidraw",
  "version": 2,
  "source": "https://github.com/zsviczian/obsidian-excalidraw-plugin/releases/tag/2.15.1",
  "elements": [
    {
      "id": "CXGAUbYb",
      "type": "text",
      "x": 0,
      "y": 0,
      "width": 17.822265625,
      "height": 25,
      "angle": 0,
      "strokeColor": "#1e1e1e",
      "backgroundColor": "transparent",
      "fillStyle": "solid",
      "strokeWidth": 2,
      "strokeStyle": "dashed",
      "roughness": 1,
      "opacity": 100,
      "groupIds": [],
      "frameId": null,
      "index": "a1",
      "roundness": null,
      "seed": 1674582751,
      "version": 7,
      "versionNonce": 37487568,
      "isDeleted": false,
      "boundElements": [],
      "updated": 1757131345421,
      "link": "[](${oldTarget})",
      "locked": false,
      "text": "⮪",
      "rawText": "⮪",
      "fontSize": 20,
      "fontFamily": 5,
      "textAlign": "left",
      "verticalAlign": "top",
      "containerId": null,
      "originalText": "⮪",
      "autoResize": true,
      "lineHeight": 1.25
    }
  ],
  "appState": {
    "theme": "light",
    "viewBackgroundColor": "#ffffff",
    "currentItemStrokeColor": "#1e1e1e",
    "currentItemBackgroundColor": "transparent",
    "currentItemFillStyle": "solid",
    "currentItemStrokeWidth": 2,
    "currentItemStrokeStyle": "dashed",
    "currentItemRoughness": 1,
    "currentItemOpacity": 100,
    "currentItemFontFamily": 5,
    "currentItemFontSize": 20,
    "currentItemTextAlign": "left",
    "currentItemStartArrowhead": null,
    "currentItemEndArrowhead": "arrow",
    "currentItemArrowType": "round",
    "currentItemFrameRole": null,
    "scrollX": 310.5888671875,
    "scrollY": 167.73828125,
    "zoom": { "value": 2 },
    "currentItemRoundness": "sharp",
    "gridSize": 20,
    "gridStep": 5,
    "gridModeEnabled": false,
    "gridColor": {
      "Bold": "rgba(217, 217, 217, 0.5)",
      "Regular": "rgba(230, 230, 230, 0.5)"
    },
    "currentStrokeOptions": null,
    "frameRendering": {
      "enabled": true,
      "clip": true,
      "name": true,
      "outline": true,
      "markerName": true,
      "markerEnabled": true
    },
    "objectsSnapModeEnabled": false,
    "activeTool": {
      "type": "selection",
      "customType": null,
      "locked": false,
      "fromSelection": false,
      "lastActiveTool": null
    }
  },
  "files": {}
}
\`\`\`
%%
`;
  }

  // ---- 旧文件（当前 Excalidraw 视图）----
  const view = ea.setView("active");
  const oldFile = view?.file;
  if (!oldFile || !oldFile.path.endsWith(".excalidraw.md")) {
    new Notice("请在 .excalidraw.md 的 Excalidraw 视图中运行脚本");
    return;
  }
  const dir = oldFile.parent?.path || "";

  // ---- 新文件名用时间戳 YYYYMMDDHHmmss ----
  const base = await uniqueBase(dir, timestampName());
  const newPath   = (dir ? `${dir}/` : "") + `${base}.excalidraw.md`;
  const futureRef = `[[${base}.excalidraw]]`;

  // ---- ① 在画布 (0,0) 插入 ⮩ 并保存 ----
  const pos = { x: 0, y: 0 };
  const id  = ea.addText(pos.x, pos.y, "⮩", { textAlign: "center" });
  ea.getElement(id).link = futureRef;
  await ea.addElementsToView(false, true);

  // ---- ② 用模板创建新文件，并在当前窗格打开 ----
  const file = await app.vault.create(newPath, buildFromTemplate(oldFile.basename));
  await app.workspace.getLeaf(false).openFile(file);

  // ---- ③ 切换到 Excalidraw 视图 ----
  await app.commands.executeCommandById("obsidian-excalidraw-plugin:toggle-excalidraw-view");

  new Notice(`已在 (0,0) 插入 ⮩，并创建并打开：${base}.excalidraw.md；新文件中央 ⮪ 已指回 ${oldFile.basename}`);
})().catch(e => {
  console.error(e);
  new Notice("脚本出错：" + e.message);
});
