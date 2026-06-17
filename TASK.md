# Vue 3 個人作品集專案實作指南

本指南根據作業規格書與畫面流程圖編寫，旨在引導你透過 **Claude Code** 逐步完成一個具備組件化、資料傳遞（Props / Emits）與路由導航功能的 Vue 3 個人作品集網站。

---

## 📌 專案核心結構建議

在開始實作前，請確保或引導 AI 將你的專案目錄結構調整為與規格書建議一致：

```text
src/
├── components/          # 區域或全域複用組件
│   ├── NavBar.vue       # 導覽列組件
│   ├── ProfileCard.vue  # 個人介紹卡片
│   ├── SkillCard.vue    # 技能卡片
│   └── ProductCard.vue  # 作品卡片（核心限制練習）
├── views/               # 頁面級組件
│   ├── HomeView.vue     # 首頁 (/)
│   ├── SkillsView.vue   # 技能頁 (/skills)
│   ├── ProjectsView.vue # 作品列表頁 (/projects)
│   └── ProjectDetailView.vue # 作品詳細頁 (/projects/:id)
├── router/
│   └── index.js         # Vue Router 路由設定
└── App.vue              # 根組件
