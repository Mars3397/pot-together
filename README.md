# PotTogether 🫕

[English](#english) | [繁體中文](#traditional-chinese)

## English

> A gamified focus‑timer web app that turns deep‑work sessions into a **virtual hot‑pot party** you can share with friends.
> Built for the course **「多媒體與人機互動總整與實作」 (Multimedia & HCI Integration & Practice, Fall 2024, NYCU)**.

---

## 🎯 Problem & Motivation

Modern students often struggle to stay focused while studying remotely. Inspired by “study with me” rooms and the communal joy of hot‑pot, **PotTogether** re‑imagines a Pomodoro‑style timer as a *collaborative cooking game*:

* Every **ingredient** represents a timed task (e.g. *25‑min tomatoes, 50‑min beef slices*).
* Teammates **join a room (pot)** and “throw” ingredients into the pot while they concentrate.
* When the timer ends, the ingredient is cooked; everyone earns XP and unlocks themed *recipes*, avatars, and limited‑edition items.

This playful metaphor encourages accountability and makes long study sessions more social and fun.

---

## ✨ Core Features

| Area                            | Highlights                                                                                                    |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| **Focus Rooms**                 | Create / join private or public rooms with configurable member limits.                                        |
| **Gamified Timer**              | Select ingredients (preset or custom duration) and track real‑time cooking progress.                          |
| **Achievements**                | Complete classic hot‑pot recipes, seasonal events, and personal streaks to level up.                          |
| **Social Feed**                 | See friends’ recent pots, cheer them on, or collaborate on *multi‑person* ingredients that shorten cook time. |
| **Responsive PWA**              | Works on desktop & mobile; installable as a lightweight app.                                                  |
| **Auth & Profiles**             | JWT‑based sign‑up / login, avatars, lifetime stats.                                                           |
| **Analytics Dashboard (Admin)** | Monitor active rooms, popular ingredients, and server health.                                                 |

---

## 🏗️ System Architecture

```
React (PWA) ──► Go API (Gin) ──► MySQL (AWS RDS)
         ▲                │
         │                └─ S3 (assets)
         └─ Docker Compose on AWS EC2
```

* **Frontend:** React 18 + Vite, MUI, React‑Game‑Engine for animated pot rendering.
* **Backend:** Go 1.22, Gin, GORM, bcrypt, JWT, REST‑style API.
* **Database:** MySQL 8 with `utf8mb4`, hosted on Amazon RDS.
* **CI/CD:** GitHub Actions ➜ Docker Hub ➜ EC2.
* **Infra:** Nginx reverse proxy, HTTPS via Let’s Encrypt, CloudWatch logs.

---

## 📜 Data Model (excerpt)

| Table        | Purpose              | Key Fields                                           |
| ------------ | -------------------- | ---------------------------------------------------- |
| `user`       | account & stats      | `id`, `username`, `email`, `level`, `total_time`     |
| `room`       | focus room           | `id`, `roomname`, `member_limit`, `current_pot`      |
| `pot`        | virtual pot instance | `id`, `room_id`, `theme`, `created_at`               |
| `ingredient` | cookable items       | `id`, `name`, `cook_time`, `xp`                      |
| `record`     | one cooking session  | `id`, `user_id`, `pot_id`, `ingredient_id`, `status` |

---

## 🔌 API Snapshot

| Method  | Route           | Action                   |
| ------- | --------------- | ------------------------ |
| `POST`  | `/users/signup` | Register & return JWT    |
| `POST`  | `/users/login`  | Login & return JWT       |
| `GET`   | `/rooms`        | List public rooms        |
| `POST`  | `/rooms`        | Create room              |
| `PATCH` | `/records/:id`  | Finish / give‑up cooking |
| `GET`   | `/ingredients`  | All ingredients          |

---

## 🗓️ Development Timeline

| Week | Milestone                                                       |
| ---- | --------------------------------------------------------------- |
| 12   | Bootstrap React repo, establish Git Flow                        |
| 13   | DB schema + initial API draft                                   |
| 14   | Core pages (home/rooms/login) + user & record APIs              |
| 15   | Progress‑update presentation – all APIs done, ingredient upload |
| 16   | 75 % feature freeze                                             |
| 17   | Final presentation & polish                                     |

---

## 👥 Team & Responsibilities

| Role                | Members            |
| ------------------- | ------------------ |
| **Full‑stack / PM** | **Yun (Mars) Kuo** |
| **Frontend**        | 江晨, 筱庭 邵           |
| **Backend**         | Chieh Yu, 陳存佩      |
| **UI/UX & Design**  | Carrie Liang       |

## 📂 Course Artifacts

* 🎞 **Final presentation slides (Canva)** — [view here](https://www.canva.com/design/DAF3H1Lcq8Q/wBBwsMRYQGTzERlGJpHDmQ/edit?utm_content=DAF3H1Lcq8Q&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)

---

## Traditional Chinese

> 一款將專注計時器變成**多人線上火鍋派對**的遊戲化 Web App。
> 本專案為 **國立陽明交通大學 2024 秋季課程「多媒體與人機互動總整與實作」** 期末作品。

---

## 🎯 問題與動機

遠距讀書的學生常難以維持專注。**PotTogether** 受「study with me」共讀室及火鍋共享氛圍啟發，將番茄鐘重新想像成「協作煮火鍋」遊戲：

* 每個**食材**代表一段任務時長（例：*25 分鐘番茄、50 分鐘牛肉*）。
* 夥伴們**加入同一鍋（房間）**，專注時把食材「丟」進鍋裡。
* 計時結束食材煮熟，全員獲得 XP，並解鎖主題*食譜*、頭像與限量物品。

這個趣味隱喻能提升彼此督促，讓長時間學習更社交、更有趣。

---

## ✨ 核心功能

| 模組          | 亮點功能                             |
| ----------- | -------------------------------- |
| **專注房間**    | 建立 / 加入私人或公開房間，可設定人數上限。          |
| **遊戲化計時器**  | 選擇食材（預設或自訂時長），即時追蹤烹煮進度。          |
| **成就系統**    | 完成經典火鍋食譜、季節活動與個人連續天數可升級。         |
| **社群動態**    | 查看朋友近期開鍋、送出鼓勵，或合作煮*多人*食材以縮短烹煮時間。 |
| **自適應 PWA** | 桌機與手機皆可用，亦可安裝為輕量 App。            |
| **帳號與個人檔案** | JWT 註冊 / 登入、頭像、自訂統計。             |
| **後台分析儀表板** | 監控活躍房間、熱門食材與伺服器健康狀態。             |

---

## 🏗️ 系統架構

```
React (PWA) ──► Go API (Gin) ──► MySQL (AWS RDS)
         ▲                │
         │                └─ S3 (資產)
         └─ Docker Compose on AWS EC2
```

* **前端：** React 18 + Vite、MUI、React‑Game‑Engine 動畫鍋渲染。
* **後端：** Go 1.22、Gin、GORM、bcrypt、JWT、REST API。
* **資料庫：** MySQL 8（`utf8mb4`），部署於 Amazon RDS。
* **CI/CD：** GitHub Actions ➜ Docker Hub ➜ EC2。
* **基礎設施：** Nginx 反向代理、Let’s Encrypt HTTPS、CloudWatch 日誌。

---

## 📜 資料模型（節錄）

| 資料表          | 目的     | 主要欄位                                                 |
| ------------ | ------ | ---------------------------------------------------- |
| `user`       | 帳號與統計  | `id`, `username`, `email`, `level`, `total_time`     |
| `room`       | 專注房間   | `id`, `roomname`, `member_limit`, `current_pot`      |
| `pot`        | 虛擬火鍋實例 | `id`, `room_id`, `theme`, `created_at`               |
| `ingredient` | 可烹煮食材  | `id`, `name`, `cook_time`, `xp`                      |
| `record`     | 單次烹煮紀錄 | `id`, `user_id`, `pot_id`, `ingredient_id`, `status` |

---

## 🔌 API 快覽

| 方法      | 路徑              | 動作        |
| ------- | --------------- | --------- |
| `POST`  | `/users/signup` | 註冊並回傳 JWT |
| `POST`  | `/users/login`  | 登入並回傳 JWT |
| `GET`   | `/rooms`        | 取得公開房間列表  |
| `POST`  | `/rooms`        | 建立房間      |
| `PATCH` | `/records/:id`  | 結束 / 放棄烹煮 |
| `GET`   | `/ingredients`  | 取得所有食材    |

---

## 🗓️ 開發時程

| 週次 | 里程碑                                         |
| -- | ------------------------------------------- |
| 12 | 建立 React 專案、確立 Git Flow                     |
| 13 | DB Schema 與初版 API 草案                        |
| 14 | 主要頁面 (home/rooms/login) + user & record API |
| 15 | 期中報告 – 全 API 完成，食材上傳                        |
| 16 | 功能凍結 75 %                                   |
| 17 | 最終展示與潤飾                                     |

---

## 👥 團隊與職責

| 角色             | 成員                 |
| -------------- | ------------------ |
| **全端 / PM**    | **Yun (Mars) Kuo** |
| **前端**         | 江晨、筱庭 邵            |
| **後端**         | Chieh Yu、陳存佩       |
| **UI/UX & 設計** | Carrie Liang       |

## 📂 課程素材

* 🎞 **期末簡報投影片 (Canva)** — [點此查看](https://www.canva.com/design/DAF3H1Lcq8Q/wBBwsMRYQGTzERlGJpHDmQ/edit?utm_content=DAF3H1Lcq8Q&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)
