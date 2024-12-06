# エンティティ定義

## 1. 製品 (Product)
### 説明
管理対象となる備品情報を保存。

### 属性
| 名前                  | データ型 | 制約                                 | 説明                                                                |
| --------------------- | -------- | ------------------------------------ | ------------------------------------------------------------------- |
| `product_id`          | TEXT     | PRIMARY KEY                          | 製品のユニークID                                                    |
| `name`                | TEXT     | NOT NULL                             | 製品名                                                              |
| `model_number`        | TEXT     |                                      | 型番                                                                |
| `owner_id`            | TEXT     | FOREIGN KEY                          | 所有者（Userへの外部キー）                                          |
| `cost`                | INT      | COST>=0                              | 製品の購入コスト                                                    |
| `purchase_date`       | DATE     | NOT NULL                             | 導入日                                                              |
| `expiration_date`     | DATE     | `expiration_date` >  `purchase_date` | 耐用期限                                                            |
| `is_remaining`        | BOOLEAN  | DEFAULT TRUE, NOT NULL               | 現存しているか(廃棄済みや失効済みならFALSE)                         |
| `purchase_request_id` | TEXT     | FOREIGN KEY                          | 追加元の購入申請（PurchaseRequestへの外部キー、寄付等の場合はNULL） |
| `remarks`             | TEXT     |                                      | 備考欄                                                              |

---
## 2. 分類 (Category)
### 説明
製品を分類するカテゴリ情報を保存。

### 属性
| 名前          | データ型 | 制約             | 説明                 |
| ------------- | -------- | ---------------- | -------------------- |
| `category_id` | TEXT     | PRIMARY KEY      | カテゴリのユニークID |
| `name`        | TEXT     | UNIQUE, NOT NULL | カテゴリ名           |
| `remarks`     | TEXT     |                  | 備考欄               |
---

## 3.製品-カテゴリ関係 (ProductCategory)
### 説明
`Product` と `Category` の多対多の関係を管理。

### 属性
| 名前          | データ型                      | 制約        | 説明                                 |
| ------------- | ----------------------------- | ----------- | ------------------------------------ |
| `product_id`  | TEXT                          | FOREIGN KEY | 製品のID（Productへの外部キー）      |
| `category_id` | TEXT                          | FOREIGN KEY | カテゴリのID（Categoryへの外部キー） |
| PRIMARY KEY   | (`product_id`, `category_id`) |             |
## 4. 部員 (User)
### 説明
システムを利用するユーザーの情報を保存。物品によっては部員の私物が管理対象となる(ボードゲーム、漫画等)ので、ユーザーの卒業日なども保存する必要がある。

### 属性
| 名前              | データ型 | 制約                    | 説明                             |
| ----------------- | -------- | ----------------------- | -------------------------------- |
| `user_id`         | TEXT     | PRIMARY KEY             | ユーザーのユニークID             |
| `handle_name`     | TEXT     | NOT NULL                | ユーザーのハンドルネーム         |
| `screen_name`     | TEXT     | UNIQUE, NOT NULL        | ユーザーのスクリーンネーム       |
| `slack_id`        | TEXT     | UNIQUE                  | ユーザーのSlack ID(通知等に使用) |
| `is_admin`        | BOOLEAN  | DEFAULT FALSE, NOT NULL | 管理者フラグ                     |
| `is_member`       | BOOLEAN  | DEFAULT TRUE, NOT NULL  | 在籍状況                         |
| `graduation_date` | DATE     |                         | 卒業日                           |
| `remarks`         | TEXT     |                         | 備考欄                           |


---

## 5. 購入申請 (PurchaseRequest)
### 説明
新しい備品の購入申請を管理。

### 属性
| 名前                  | データ型 | 制約                 | 説明                                                                        |
| --------------------- | -------- | -------------------- | --------------------------------------------------------------------------- |
| `purchase_request_id` | TEXT     | PRIMARY KEY          | 申請のユニークID                                                            |
| `applicant_id`        | TEXT     | FOREIGN KEY          | 申請者のID（Userへの外部キー）                                              |
| `product_name`        | TEXT     | NOT NULL             | 申請された製品名                                                            |
| `model_number`        | TEXT     |                      | 型番                                                                        |
| `cost`                | INT      | COST>=0              | 申請された購入費用                                                          |
| `status_id`           | TEXT     | FOREIGN KEY          | 購入申請状態ID（Statusへの外部キー、pending(保留)やapproved(承認済み)など） |
| `request_date`        | DATE     | DEFAULT CURRENT_DATE | 申請作成日                                                                  |
| `approval_date`       | DATE     |                      | 承認日                                                                      |
| `product_url`         | TEXT     |                      | 購入希望商品のURL                                                           |
| `remarks`             | TEXT     |                      | 備考欄                                                                      |

---
## 6. 購入申請の状態 (PurchaseRequestStatus)
### 説明
購入申請の状態一覧を管理。

### 属性
| 名前                         | データ型 | 制約             | 説明                      |
| ---------------------------- | -------- | ---------------- | ------------------------- |
| `purchase_request_status_id` | TEXT     | PRIMARY KEY      | 状態のユニークID          |
| `status_name`                | TEXT     | UNIQUE, NOT NULL | 状態名（例: Pendingなど） |
| `remarks`                    | TEXT     |                  | 備考欄                    |




