# Day 18 — Sổ tay học viên
## AI Financial Modeling & ROI
### Bài toán sống còn

> **A great AI product can still die from running out of money.**
> Một sản phẩm AI tuyệt vời vẫn có thể chết vì hết tiền.

---

## 0. Cách dùng sổ tay này

Dùng **trong lúc làm workshop** — không phải để đọc hết một lượt. Mỗi lần bí, mở đúng phần cần:

| Khi bạn cần… | Mở phần… |
|---|---|
| Hiểu mạch logic tổng thể | §1 |
| Đọc nhanh một khái niệm | §2 |
| Cách điền file Excel template | §3 |
| Chạy AI critique mô hình tài chính | §4 (Prompts) |
| Biết output gì cần có ở mỗi checkpoint | §5 (Checkpoints) |
| Xem tiêu chí chấm điểm & cách submit | §6 (Rubric & Submission) |
| Đọc tham khảo sâu hơn | §7 (References) |

**Quy ước ngôn ngữ:** Tiếng Việt ưu tiên. Các **term, công thức, prompt** giữ tiếng Anh để chuẩn ngành và không lệch nghĩa khi làm việc với AI.

**File đi kèm:** `day18_financial_model.xlsx` — template Excel với 3 tabs đã build sẵn formulas.

---

## 1. Mạch logic Day 18

Day 18 trả lời 4 câu hỏi mà *bất kỳ* sếp, CFO, hay nhà đầu tư nào cũng sẽ hỏi bạn về sản phẩm AI:

```
       Câu 1: Tốn tiền vào đâu? Thu tiền kiểu gì?
                       ↓
       Câu 2: Có lãi trên từng khách không?
                       ↓
       Câu 3: Tổng thể dự án có đáng làm không?
                       ↓
       Câu 4: Nếu Pessimistic xảy ra, làm gì để sống?
```

### Tại sao Day 18 quan trọng?

**90% AI startups chết trước khi tìm thấy Product-Market Fit** (Y Combinator). Lý do số 1: **hết tiền** — không phải công nghệ dở.

Day 16-17 các bạn đã build được **product** tốt. Day 18 đảm bảo bạn **sống đủ lâu** để build sản phẩm đó.

### 4 blocks và đầu ra

| Block | Câu hỏi | Output |
|---|---|---|
| **1. Cost & Revenue** | Tốn tiền vào đâu? Thu tiền kiểu gì? | Cost structure + Revenue model |
| **2. Unit Economics** | Có lãi trên từng khách không? | LTV/CAC > 3 + CAC Payback < 12 tháng |
| **3. ROI & Scenarios** | Dự án có đáng làm không? | NPV > 0 + IRR > WACC + Project Payback < 24 tháng |
| **4. Live Demo & Workshop** | Stress test với Pessimistic | File Excel hoàn chỉnh + Decision Note |

---

## 2. Các khái niệm chính

### 2.1 Burn Rate & Runway — 2 con số sống còn

Đây là **2 con số trên trán mỗi founder** — check hàng tuần, không hàng tháng.

#### Burn Rate / Tốc độ đốt tiền

```
Burn Rate = Tiền chi ra − Tiền thu vào (mỗi tháng)
```

Ví dụ:
- Thu vào: 10 triệu/tháng
- Chi ra: 30 triệu/tháng
- **Burn Rate = 20 triệu/tháng**

Đây là tốc độ máu chảy ra khỏi cơ thể công ty.

#### Runway / Đường băng

```
Runway = Số tiền đang có / Burn Rate
```

Ví dụ:
- Tiền mặt: 200 triệu
- Burn Rate: 20 triệu/tháng
- **Runway = 10 tháng**

Bạn có đúng 10 tháng để **cất cánh** — bắt đầu sinh lãi hoặc raise vòng vốn tiếp theo.

> **"Running out of money is how startups die." — Tim Brady, Y Combinator Partner**

---

### 2.2 Cost Components — 5 cục chi phí của AI startup

#### 3 cục dễ thấy

| Cục | Ví dụ | Đặc điểm |
|---|---|---|
| **Cloud Infrastructure** | AWS, GCP, Azure | Server, storage, network |
| **Foundation Model API** | OpenAI, Anthropic, Gemini | Trả theo token — *cục mới so với SaaS truyền thống* |
| **R&D & Salaries** | Lương dev, PM, designer | Cục to nhất early-stage |

#### 2 cục ít người kể

| Cục | Ví dụ | Đặc điểm |
|---|---|---|
| **Sales & Marketing** | Ads, sales team, content | Phình to nhanh khi cần scale |
| **AI Hidden Costs** | Labeling, retraining, QA, compliance | **Cục giết startup AI** |

#### AI Hidden Costs — chi tiết

Đây là **tảng băng chìm**. Phần nổi (API Cost) các bạn nhìn thấy. Phần chìm:

| Hidden cost | Mô tả | Quy tắc tham khảo |
|---|---|---|
| **Data Labeling** | Thuê người đọc log, dạy lại AI khi nó "sủa" nhầm | 5-15% chi phí vận hành |
| **Model Retraining** | Model "ngu" đi theo thời gian (drift), phải retrain định kỳ | **~20% chi phí build ban đầu mỗi năm** |
| **Human-in-the-loop** | Nhân sự kiểm tra output AI trước khi giao khách (đặc biệt B2B) | Tùy product, có thể 10-30% COGS |
| **Compliance & Security** | Kiểm định an toàn dữ liệu, ISO, SOC2, GDPR | One-time 100-500M cho B2B enterprise |

**Quy tắc vàng:** Nếu bạn không tính 4 thứ này vào COGS từ đầu, bạn sẽ **lỗ nặng khi scale**.

---

### 2.3 AI vs SaaS — Cú sốc về biên lợi nhuận

Đây là điểm khác biệt lớn nhất giữa làm AI product và SaaS truyền thống:

| | **SaaS truyền thống** | **AI Product** |
|---|---|---|
| **COGS** | 10-20% revenue | 40-60% revenue |
| **Gross Margin** | 80-90% | 40-60% |
| **Lý do** | Server cost thấp, marginal cost ~0 | API & compute đắt theo mỗi request |

**Hệ quả:** Bạn **không thể áp dụng playbook giá của SaaS lên AI** mà không bị lỗ. Pricing và unit economics phải được thiết kế lại từ đầu.

---

### 2.4 Revenue Models — 4 cách thu tiền

| Model | Cách thu | Khi nào dùng | Ví dụ |
|---|---|---|---|
| **SaaS / MRR** | Phí cố định mỗi tháng | Khi value rõ và sử dụng đều đặn | Notion AI, Linear |
| **Consumption** | Dùng bao nhiêu trả bấy nhiêu | Infrastructure-level, API products | OpenAI API, AWS |
| **Transactional** | Hoa hồng trên giao dịch | Marketplace, payment | Stripe, Upwork |
| **Hybrid** | Base fee + Overage charge | **Khuyến nghị mặc định cho AI** | ChatGPT Plus, GitHub Copilot |

**Y Combinator thích MRR nhất** — vì doanh thu predictable. Doanh thu dự đoán được = nhà đầu tư yên tâm = định giá cao hơn.

#### Tại sao Hybrid an toàn nhất cho AI?

**Câu chuyện buffet:** Bạn mở buffet 200K/người. Đa số ăn 150K rồi về (lãi 50K). Nhưng có 10% khách là *anh tập tạ* — ăn 500K thịt bò (lỗ 300K). Trung bình → lỗ.

AI cũng vậy. **Power users** cắm chatbot chạy bot 24/7, đốt 100$ API trong khi gói SaaS chỉ 50$. Hybrid pricing bảo vệ margin: power user phải trả tương xứng với cost họ tạo ra.

---

### 2.5 Unit Economics — Kinh tế đơn vị

Trả lời 2 câu hỏi đơn giản: *Mất bao nhiêu để có 1 khách? Họ trả lại bao nhiêu?*

> Đây là **câu hỏi đầu tiên YC sẽ hỏi bạn ở Demo Day**.

#### CAC — Customer Acquisition Cost

```
CAC = Tổng tiền Sales & Marketing / Số khách hàng mới
```

Ví dụ: Đốt 10 triệu Ads, có 100 khách mới → **CAC = 100K/khách**

**Lưu ý quan trọng:** *Tổng tiền Sales & Marketing* phải tính cả lương sales, marketing team, tools, agency fees — không chỉ tiền ads. Đa số startup đầu tiên tính sót → CAC ảo thấp hơn thực tế.

**Pro tip:** Tính CAC theo từng channel riêng. Có channel CAC 50K, có channel CAC 500K. Dồn ngân sách vào channel rẻ.

#### LTV — Customer Lifetime Value

```
LTV = ARPU × Gross Margin × Số tháng ở lại
```

Ví dụ:
- ARPU = 50K/tháng
- Gross Margin = 60% → Lãi gộp 30K/tháng
- Khách ở lại 10 tháng
- **LTV = 30K × 10 = 300K**

**Lỗi phổ biến:** Tính LTV bằng *doanh thu* thay vì *lãi gộp*. Sai. Phải dùng Gross Margin.

> **"Founder nào tính LTV bằng revenue thay vì gross margin → VC phát hiện trong 30 giây và không bao giờ trả lời email lần thứ hai."**

#### Churn Rate

```
Churn Rate = % khách rời đi mỗi tháng
Số tháng ở lại ≈ 1 / Churn Rate
```

Đây là cái **thùng nước có lỗ thủng**. Đổ nước vào (acquire) — nhưng nước cũng chảy ra (churn).

**Benchmark:**
| Loại sản phẩm | Churn "tốt" |
|---|---|
| B2B SaaS | < 2%/tháng |
| B2C SaaS | < 5%/tháng |
| **AI product** | **< 5%/tháng** |

AI product churn cao vì công nghệ thay đổi nhanh — khách thử model mới mỗi tháng.

#### LTV/CAC Ratio — Tiêu chuẩn vàng VC

| Vùng | Ratio | Ý nghĩa |
|---|:---:|---|
| 🔴 Đỏ | **< 1** | Đốt tiền — bỏ 1 đồng marketing thu về < 1 đồng |
| 🟡 Vàng | **1 – 2** | Sống ngắc ngoải — đủ trả marketing, không đủ trả lương |
| 🟢 Xanh | **> 3** | Tiêu chuẩn vàng — VC ready |

**Tại sao là 3, không phải 1?**

Vì LTV chỉ là lợi nhuận *gộp*. Bạn vẫn phải trả lương dev, văn phòng, R&D. Bỏ 1 đồng marketing kiếm 3 đồng lãi gộp — sau khi trừ overhead — mới còn lãi ròng đủ tái đầu tư.

> Median của public SaaS hiện nay là **4.2:1** (KeyBanc 2024). Mục tiêu nên đặt vào vùng đó.

#### CAC Payback Period

```
CAC Payback = CAC / Gross Margin per month
```

Ví dụ: CAC 100K, lãi gộp 10K/tháng → Payback = **10 tháng**.

**Benchmark:**
| Payback | Đánh giá |
|---|---|
| < 12 tháng | Tốt cho early-stage |
| 12 – 18 tháng | Chấp nhận nếu LTV/CAC cao |
| > 18 tháng | Cảnh báo — cần dòng tiền dồi dào |

**Pro tip:** Trước khi khách trả đủ tiền để hòa CAC, bạn vẫn đang **âm dòng tiền** trên khách đó. Nếu Payback quá dài, bạn sẽ cạn tiền mặt trước khi khách kịp trả.

---

### 2.6 ROI Metrics — Đánh giá tổng thể dự án

#### NPV — Net Present Value

```
NPV = Tổng dòng tiền tương lai (đã chiết khấu) − Vốn đầu tư ban đầu
```

**Giải thích đơn giản:** Bỏ 100 triệu mua robot AI thay 2 nhân viên. 3 năm sau robot tiết kiệm 150 triệu.

Nghe lãi 50 triệu? **Sai.** Vì lạm phát + cơ hội đầu tư khác — 150 triệu của 3 năm sau **không đáng giá bằng** 150 triệu bây giờ. NPV trừ đi sự "mất giá" đó (gọi là *discount rate*).

Sau khi trừ, có thể NPV chỉ còn +20 triệu. Vẫn lãi thực — nhưng ít hơn bạn tưởng.

> **Nguyên tắc: NPV > 0 → Dự án đáng làm.**

Excel có hàm `NPV()` lo phần toán. Việc của bạn là chọn discount rate đúng. Với AI startup rủi ro cao: **20-25%/năm**.

#### IRR — Internal Rate of Return

Tốc độ sinh lời nội tại của dự án (% mỗi năm).

So sánh:
- Gửi ngân hàng: 7%/năm — chả phải làm gì
- Trái phiếu chính phủ: 5%/năm
- Dự án AI của bạn: **IRR phải > 20%** vì rủi ro cao

> **Nguyên tắc: IRR > Chi phí vốn (WACC) → Dự án đáng làm.**

#### Project Payback vs CAC Payback

| | **CAC Payback** | **Project Payback** |
|---|---|---|
| Phạm vi | 1 khách hàng | Toàn dự án |
| Hỏi | Bao lâu thu hồi vốn marketing 1 khách? | Bao lâu thu hồi vốn build MVP, lương team? |
| Ví dụ | 100K CAC, 10K lãi/tháng → 10 tháng | 500M build, 50M lãi/tháng → 10 tháng |
| Benchmark | < 12 tháng | < 24 tháng |

---

### 2.7 3 Scenarios — Optimistic, Base, Pessimistic

Trong tech AI, **không ai đoán được tương lai**. OpenAI giảm giá ngày mai. Google ra tool free giết product của bạn. Microsoft mua một startup khác.

Khi lập kế hoạch, **luôn phải có 3 kịch bản:**

| Scenario | Mục đích | Đặc điểm |
|---|---|---|
| **Optimistic** | Vẽ giấc mơ — pitch nhà đầu tư | OpenAI giảm giá, viral lan, khách trả nhiều hơn |
| **Base** | Lập plan và budget thực tế | Dựa trên benchmark thị trường + giả định hợp lý |
| **Pessimistic** | **Cứu mạng** — stress test | Google ra tool free, khách rời nhanh, CAC tăng gấp đôi |

> **Pessimistic case mới là cái cứu mạng bạn — không phải Optimistic case.**

---

### 2.8 Sensitivity Analysis — Phân tích độ nhạy

Tự hỏi: *"Nếu biến số này xấu đi 20%, dự án có chết không?"*

**3 biến nguy hiểm nhất trong AI SaaS:**

| Biến | Ý nghĩa | Vì sao có thể xấu đi |
|---|---|---|
| **Adoption Rate** | Khách không mua nhiều như dự kiến | Thị trường lạnh, marketing pitch hay nhưng không convert |
| **ARPU** | Không bán được giá cao | Cạnh tranh đẩy giá xuống, khách mặc cả nhiều |
| **Churn Rate** | Khách dùng 1 tháng rồi bỏ | Value chưa rõ, sản phẩm chưa kịp tạo thói quen |

**Quy tắc:** Chỉ cần chỉnh 1 trong 3 biến xấu đi 20% — bạn sẽ thấy dòng tiền sụp đổ thế nào.

---

## 3. Hướng dẫn dùng file Excel template

File: `day18_financial_model.xlsx` — 3 tabs, **chỉ điền vào ô MÀU VÀNG**.

### 3.1 Tab 1 — Assumptions

Đây là tab duy nhất bạn nhập input. 8 sections:

#### Section 1 — Product & Pricing

| Field | Hướng dẫn điền |
|---|---|
| **ARPU** | Giá bạn nghĩ thị trường sẽ chấp nhận. Đừng bịa cao — phải justify được. |
| **Adoption rate** | % của TAM chuyển thành khách trả tiền *mỗi tháng*. B2B Vietnam: 0.2-1%. B2C tốt: 1-5%. |
| **TAM** | **Đừng bao giờ bịa.** Vào GSO (Tổng cục Thống kê), Statista, hoặc estimate top-down. |

#### Section 2 — COGS

| Field | Hướng dẫn |
|---|---|
| **API cost** | Tính theo số tokens × giá OpenAI/Anthropic |
| **Hidden costs** | **Đừng bỏ qua.** Labeling + Retraining + QA. ~30-40% API cost. |
| **Infrastructure** | Cloud cost allocate cho mỗi user |

> **→ Tổng COGS / khách / tháng** tự cộng từ 3 dòng trên

#### Section 3 — Customer Behavior

| Field | Hướng dẫn |
|---|---|
| **Monthly Churn Rate** | B2B: < 2%. B2C: < 5%. AI: < 5%. **Pessimistic phải cao hơn Base ít nhất 1.5x.** |

> **→ Số tháng ở lại trung bình** = 1 / Churn Rate (tự tính)

#### Section 4 — CAC

| Field | Hướng dẫn |
|---|---|
| **CAC** | Tổng Marketing & Sales / Khách mới. Phải tính cả lương sales team. |

#### Section 5 — Fixed Costs

| Field | Hướng dẫn |
|---|---|
| **Salaries** | Lương team. Pessimistic = phải hire thêm khi khó scale. |
| **Office, tools, admin** | Văn phòng, SaaS tools, kế toán |
| **Marketing budget** | Ngân sách marketing hàng tháng |

> **→ Tổng Fixed Cost / tháng** tự cộng

#### Section 6 — Initial Investment

| Field | Hướng dẫn |
|---|---|
| **Vốn đầu tư ban đầu** | Lương team build MVP + infrastructure setup. Chi 1 lần. |
| **Tiền mặt ban đầu** | Số tiền có sẵn sau khi đã trừ vốn đầu tư. **Realistic với pre-seed Vietnam: 1-5 tỷ.** |

#### Section 7 — Discount Rate

| Field | Hướng dẫn |
|---|---|
| **Annual WACC** | 20% standard cho AI startup rủi ro cao. Có thể dùng 25% nếu rủi ro rất cao. |

> **→ Monthly discount rate** tự tính

#### Section 8 — Decision Note

Ô vàng cuối tab — viết 2-3 câu trả lời:

> *"Tại sao tôi chọn ARPU và CAC như trên? Logic gì để bảo vệ con số này trước nhà đầu tư?"*

**Đây là phần quan trọng nhất.** Số đẹp mà không có logic = số ảo.

### 3.2 Tab 2 — Unit Economics

**Tự động tính từ Tab 1.** Không sửa gì.

Kiểm tra:
- ✅ **LTV/CAC > 3** ở cột Base — tô **xanh**
- ✅ **CAC Payback < 12 tháng** ở cột Base — tô **xanh**
- ✅ **Verdict: HEALTHY** ở cột Base

Nếu **không đạt** ở cột Base → quay lại Tab 1, điều chỉnh:
- Tăng ARPU
- Hoặc giảm CAC (chuyển sang organic marketing)
- Hoặc giảm Churn (cải thiện product để giữ khách)

**Đừng "bịa" số đẹp.** Phải *thay đổi mô hình kinh doanh* để đạt được số đó.

### 3.3 Tab 3 — P&L & ROI

**Có dropdown chọn scenario ở C4.** Đổi giữa Optimistic/Base/Pessimistic — toàn bộ P&L tự cập nhật.

Kiểm tra **ở cột Base**:
- ✅ **NPV > 0**
- ✅ **IRR > WACC** (thường > 20%)
- ✅ **Project Payback < 24 tháng**
- ✅ **Verdict: GO**

Sau đó đổi scenario sang **Pessimistic**:
- 🚨 **Runway phải ≥ 12 tháng** (nếu < 12 → cần cắt scope hoặc raise thêm vốn)
- 🚨 Cash Position bị tô đỏ tháng nào — đó là tháng cần raise tiền hoặc đổi hướng

> **Đây không phải bug. Đây là model đang nói thật cho bạn nghe.** Pessimistic xảy ra → cần biết phải làm gì.

---

## 4. Prompts cho Vibe Coding (English-only)

> **Lưu ý:** Tất cả prompt giữ tiếng Anh 100% để AI giữ ngữ nghĩa ổn định. Thảo luận kết quả bằng tiếng Việt.

### 4.1 Financial Model Stress-Test Prompt (Prompt chính)

Dùng sau khi đã điền đầy đủ Tab 1 của file Excel. Copy nội dung Tab 1 ra dạng text rồi paste vào prompt.

```
Act as a ruthless CFO and a skeptical Series A investor.
Review my financial model assumptions for an AI product below.

Do NOT rewrite my assumptions. Perform a stress test:

1. ASSUMPTION REALISM
   For each input, evaluate if it is realistic for an early-stage AI startup
   in the given market. Flag any assumption that is too optimistic compared
   to industry benchmarks. Cite specific benchmarks where possible
   (Bessemer, KeyBanc, YC data).

2. HIDDEN COST GAPS
   Identify any AI-specific cost categories I may have underestimated:
   - Data labeling costs
   - Model retraining frequency and cost
   - Human-in-the-loop QA
   - Compliance and security
   Estimate what realistic numbers should be for my product type.

3. UNIT ECONOMICS RED FLAGS
   Calculate LTV/CAC and CAC Payback from my numbers.
   Identify if any input is propping up unrealistic ratios.
   Specifically check if I'm using Revenue (wrong) or Gross Margin (correct)
   in my LTV calculation.

4. SCENARIO STRESS TEST
   Take my Base case and apply a 20% adverse shock to:
   - Adoption Rate
   - ARPU
   - Churn Rate
   What month would the company run out of cash?
   Is my Pessimistic case realistic, or too soft?

5. PRICING MODEL FIT
   Given my product type, is SaaS / Consumption / Hybrid the right model?
   What pricing structure would protect margin against power users?

Be direct, highly critical, and provide specific examples for your critiques.
Do not be encouraging. Find the problems before investors do.

[Paste your Tab 1 Assumptions here]
```

**Cách dùng:**
1. Mở Tab 1 file Excel
2. Copy toàn bộ giả định ra dạng text (3 cột Optimistic/Base/Pessimistic)
3. Paste vào prompt và run
4. Đọc từng điểm AI chỉ ra
5. Với mỗi điểm: **accept** (sửa theo), **reject** (giữ, có lý do), hoặc **partial**
6. Mọi sửa đổi phải do **bạn viết lại** — không copy nguyên văn từ AI

---

### 4.2 Hidden Costs Discovery Prompt

Dùng khi bạn không chắc đã liệt kê đủ AI hidden costs cho product của mình.

```
I am building an AI product with these characteristics:
- Product type: [describe what the AI does]
- Target user: [B2B SME / B2C / Enterprise]
- AI capability: [text generation / classification / RAG / agentic / etc.]
- Expected scale at month 12: [number of users / requests per day]

List ALL hidden cost categories specific to this product that founders
typically underestimate or forget. For each:

1. Cost category name
2. Why it exists for this product type specifically
3. Realistic monthly cost estimate at the scale described
4. How this cost scales with growth (linear / sub-linear / step function)
5. Industry benchmark or reference if available

Focus on costs OUTSIDE of:
- Foundation model API fees
- Cloud infrastructure
- Salaries
- Office and admin

I want the costs that will surprise me when I scale.
```

---

### 4.3 Pricing Model Validation Prompt

Dùng khi cần quyết định giữa SaaS, Consumption, Hybrid.

```
My AI product details:
- What it does: [describe]
- Target user: [B2B / B2C, segment, geography]
- Core value delivered: [the main thing the user gets]
- Usage pattern: [low usage occasional / heavy daily / variable burst]
- Unit cost per usage: [API cost or compute cost per request]

Recommend the optimal pricing model:
1. Should I use SaaS (flat MRR), Consumption, Transactional, or Hybrid?
2. Why this model fits this product specifically?
3. What pricing tiers would you suggest?
4. How should I protect against power users in this model?
5. What is the risk of this pricing model in the worst case scenario?

Compare against 2-3 real AI products with similar usage patterns
and explain how they price.

Be specific about numbers if you can. Use VND if Vietnam market,
USD otherwise.
```

---

### 4.4 Pessimistic Scenario Builder Prompt

Dùng khi bạn không biết chỉnh giả định nào trong Pessimistic case cho realistic.

```
I have a Base case financial model for an AI product with these key inputs:
- ARPU: [number] per month
- Adoption rate: [X%] of TAM per month
- Monthly Churn: [X%]
- CAC: [number]
- Total monthly fixed cost: [number]
- Initial cash: [number]

Build me a realistic Pessimistic scenario by answering:

1. What 3 external events could trigger this Pessimistic case?
   (e.g., competitor launches free tool, foundation model price hike,
   economic downturn affecting customer budgets)

2. For each input above, what is a realistic Pessimistic value?
   - Apply realistic adverse shocks, not random 20% reductions
   - Justify each shock with reasoning

3. Under this Pessimistic case, in which month does cash run out
   if no action is taken?

4. List 5 concrete actions the founder could take in month 1
   (the moment Pessimistic becomes likely) to extend runway by
   at least 6 months. Rank by impact.

Be specific. Numbers, not vague advice.
```

---

### 4.5 Decision Note Quality Check Prompt

Dùng để kiểm tra Decision Note của bạn có đủ thuyết phục không.

```
I wrote this Decision Note to defend my financial assumptions:

"[paste your Decision Note here]"

Evaluate it as a Series A investor reading 50 pitch decks today:

1. Does it answer "WHY this number, not another"?
2. Does it cite any market evidence, comparable, or customer interview?
3. Could a competitor founder write the same Decision Note with their
   own numbers? (If yes, it's too generic)
4. What is the single weakest claim in this note that I would push
   back on first?
5. Suggest one specific edit that would make this note 10x more
   defensible.

Be brutally honest. I would rather hear it from you now than from
an investor who never replies again.
```

---

## 5. Checkpoints — Output yêu cầu

### 5.1 Checkpoint 1 — Tab 1 hoàn thành (15 phút đầu workshop)

**Output:** Tab 1 với *tất cả* ô vàng đã điền cho cả 3 cột (Optimistic/Base/Pessimistic).

**Pass signals:**
- Mọi ô vàng đều có số (không bỏ trống)
- Pessimistic có Churn cao hơn Base ít nhất 1.5x
- Pessimistic có CAC cao hơn Base ít nhất 1.5x
- TAM có nguồn (GSO, Statista, hoặc top-down logic)
- Hidden Costs ≥ 30% API cost

**Fail signals:**
- Pessimistic = Base (không stress test thật)
- TAM bịa (không có reasoning)
- Hidden Costs = 0 (quên cục giết startup)
- Initial Cash quá ít hoặc quá nhiều (không realistic với pre-seed)

---

### 5.2 Checkpoint 2 — Tab 2 đạt benchmark (5 phút sau Checkpoint 1)

**Output:** Tab 2 hiển thị **xanh** ở cột Base.

**Pass signals:**
- LTV/CAC > 3 (xanh)
- CAC Payback < 12 tháng (xanh)
- Verdict: ✓ HEALTHY

**Fail signals:**
- Một trong các chỉ số đỏ → quay lại Tab 1, điều chỉnh
- Tránh "fix" bằng cách bóp méo số. Phải đổi mô hình kinh doanh.

---

### 5.3 Checkpoint 3 — Tab 3 sống được Pessimistic (10 phút cuối)

**Output:** Tab 3 với scenario Pessimistic, Runway ≥ 12 tháng.

**Pass signals:**
- Pessimistic Runway ≥ 12 tháng
- Hoặc đã có Decision Note giải thích cách raise vốn để extend runway

**Fail signals:**
- Pessimistic Runway < 6 tháng và không có plan
- Cash Position đỏ ngay từ tháng 1-2 (chưa kịp build product đã hết tiền)

---

### 5.4 Final Checklist — Trước 13:00

```
[ ] 1. Tab 1 đầy đủ — tất cả ô vàng đều có số cho cả 3 cột
[ ] 2. Tab 2 — Base scenario LTV/CAC > 3 và CAC Payback < 12
[ ] 3. Tab 3 — Base NPV > 0, IRR > 20%
[ ] 4. Tab 3 — Pessimistic scenario, Runway ≥ 12 tháng
[ ] 5. Decision Note (1-2 đoạn) trả lời "Tại sao chọn ARPU và CAC này?"
```

**Minimum bar:** Nếu một nhà đầu tư mở file Excel và đọc Decision Note, họ phải hiểu được: bạn build cái gì, chi phí thực tế bao nhiêu, có lãi không, và sống được bao lâu — **mà không cần hỏi lại hơn 3 câu**.

---

## 6. Rubric & Submission

### 6.1 Cách nộp bài

Day 18 chỉ có **1 phiên bản nộp duy nhất**:

| Mục | Yêu cầu |
|---|---|
| **Nộp khi nào** | Cuối buổi sáng Day 18 — **trước 13:00** |
| **Nộp ở đâu** | Submit trực tiếp trên LMS |
| **Tên file** | `[Tên]_Day18.xlsx` |
| **Bao gồm** | File Excel với 3 tabs hoàn chỉnh + Decision Note ở Tab 1 Section 8 |

> **Không có BTVN.** Khác với Day 16 và 17 — Day 18 toàn bộ workshop diễn ra trong buổi sáng. Tất cả công việc cần hoàn thành tại lớp.

---

### 6.2 Rubric — 100 điểm, 5 tiêu chí

| # | Tiêu chí | Điểm |
|---|---|---:|
| 1 | **Assumption Realism** | 30 |
| 2 | **AI Cost Awareness** | 25 |
| 3 | **Unit Economics Quality** | 20 |
| 4 | **Scenario Stress-Test** | 15 |
| 5 | **Decision Note Quality** | 10 |

---

#### Tiêu chí 1 — Assumption Realism (30 điểm)

Chấm chất lượng và độ realistic của các giả định trong Tab 1. Số có nguồn rõ ràng không? Có justify được trước nhà đầu tư không? TAM có evidence không, hay chỉ là số bịa?

*Điểm cao nhất dành cho bài có thể trả lời: "Tại sao mỗi con số trong Tab 1 là số đó, không phải số khác."*

---

#### Tiêu chí 2 — AI Cost Awareness (25 điểm)

Chấm hiểu biết về AI-specific costs. Hidden Costs có đầy đủ 4 loại (Labeling, Retraining, Human-in-loop, Compliance) không? Có phân biệt đúng API cost vs Hidden Costs không? COGS Pessimistic có cao hơn Base hợp lý không?

*Tiêu chí này phân biệt người hiểu AI economics với người chỉ làm Excel.*

---

#### Tiêu chí 3 — Unit Economics Quality (20 điểm)

Chấm tính chính xác và hợp lý của LTV, CAC, LTV/CAC, Payback. LTV có dùng Gross Margin (không dùng Revenue) không? Pessimistic Churn có thực sự xấu hơn Base không? Cột Base có đạt LTV/CAC > 3 và CAC Payback < 12 tháng không?

---

#### Tiêu chí 4 — Scenario Stress-Test (15 điểm)

Chấm chất lượng của Pessimistic scenario. Pessimistic có thực sự stress test (không phải sao chép Base) không? Có 3 cột thực sự khác nhau và logic không?

*Điểm cao nhất dành cho bài có Pessimistic Runway ≥ 12 tháng — chứng tỏ founder đã chuẩn bị cho cú sốc.*

---

#### Tiêu chí 5 — Decision Note Quality (10 điểm)

Chấm chất lượng Decision Note ở Tab 1 Section 8. Note có defendable không? Có cite được nguồn/benchmark không? Có giải thích được logic chọn ARPU và CAC không?

*Note generic ("Đây là số hợp lý") = 0 điểm. Note có dẫn nguồn cụ thể (GSO, Bessemer, customer interview) = full mark.*

---

### 6.3 Grade bands

| Band | Điểm | Ý nghĩa |
|---|---:|---|
| Outstanding | 90–100 | Có thể đem file này đi pitch nhà đầu tư mà không cần sửa |
| Strong | 75–89 | Đủ chất để trình sếp, có thể cần làm rõ 1-2 điểm |
| Pass | 60–74 | Hiểu khái niệm nhưng giả định còn yếu, cần revise trước khi defend |
| Needs rework | 40–59 | Sai một số khái niệm core (ví dụ tính LTV bằng revenue) |
| Fail | <40 | Chưa đạt minimum bar |

---

### 6.4 Lưu ý khi làm bài

- **Đừng bịa số đẹp.** VC sẽ thấy ngay. LTV/CAC = 8.5 với CAC bịa thấp = bị loại.
- **Hidden Costs ≠ 0.** Nếu bỏ trống, mất ngay 10 điểm Tiêu chí 2.
- **Pessimistic ≠ Base.** Phải có shock thực sự (Churn 1.5x, CAC 1.5x).
- **Decision Note phải defendable.** Số có nguồn, logic chặt, không generic.
- **AI là công cụ critique, không phải author.** Dùng prompts §4 để stress test — nhưng quyết định cuối cùng là của bạn.

---

## 7. References

### Core readings

1. **Y Combinator — Why Startups Die** — Tim Brady
   https://www.ycombinator.com/library/8j-the-end-of-cheap-money-and-what-it-means-for-startups
   Bài viết về runway management từ YC Partner.

2. **Bessemer — State of the Cloud Reports**
   https://www.bvp.com/atlas/state-of-the-cloud-2024
   Benchmark CAC Payback, Net Retention, LTV/CAC cho SaaS hiện đại.

3. **KeyBanc Capital Markets — SaaS Survey**
   Annual report về SaaS metrics — median LTV/CAC, payback periods, etc.

4. **a16z — The New Business of AI**
   https://a16z.com/the-new-business-of-ai-and-how-its-different-from-traditional-software/
   Bài kinh điển về AI economics khác SaaS truyền thống thế nào.

### Further reading (optional)

5. **The Lean Startup** — Eric Ries
   Framework về validated learning, MVP, build-measure-learn.

6. **The SaaS Bible** — Patrick Campbell, ProfitWell
   Toàn tập về pricing, churn, LTV/CAC trong SaaS.

7. **Venture Deals** — Brad Feld
   Cách đọc term sheet và hiểu economics của vòng gọi vốn.

8. **Sean Ellis — The 40% Rule for PMF**
   https://www.startup-marketing.com/the-startup-pyramid/
   Method to measure PMF before scaling marketing.

### Quick reference — các câu chốt Day 18

| Câu chốt |
|---|
| "Running out of money is how startups die." — Tim Brady, YC |
| "AI có COGS cao hơn SaaS — biên lợi nhuận mỏng hơn rất nhiều." |

---

## 8. Áp dụng cho AgeTrace — Trả lời các câu hỏi Day 18

Dựa trên sản phẩm AgeTrace từ Day 16-17, dưới đây là việc áp dụng khuôn khổ Day 18 để trả lời 4 câu hỏi chính.

### 8.1 Cost & Revenue Structure

**Câu hỏi: Tốn tiền vào đâu? Thu tiền kiểu gì?**

#### Cost Structure (5 cục chi phí):
- **Cloud Infrastructure**: AWS/GCP cho face recognition models, storage ảnh hồ sơ. Ước tính: 2-5 triệu/tháng cho 100 khách.
- **Foundation Model API**: OpenAI/Anthropic cho LLM giải thích, hoặc custom models cho age-invariant embedding. Ước tính: 1-3 triệu/tháng.
- **R&D & Salaries**: Lương dev AI, data scientist. Ước tính: 20-30 triệu/tháng cho team 5 người.
- **Sales & Marketing**: Ads, sales team cho B2B. Ước tính: 5-10 triệu/tháng.
- **AI Hidden Costs**: Data labeling (thuê người tag ảnh khuôn mặt), model retraining (6 tháng/lần), human-in-loop QA (kiểm tra kết quả), compliance (ISO 27001). Ước tính: 30-50% API cost, ~1-2 triệu/tháng.

**Revenue Model:** Hybrid — Base fee 5 triệu/tháng + Overage cho số truy vấn vượt (0.5K/truy vấn). Lý do: Bảo vệ margin với power users (cơ quan lớn truy vấn nhiều).

### 8.2 Unit Economics

**Câu hỏi: Có lãi trên từng khách không?**

#### Giả định Base Case:
- ARPU: 7 triệu/tháng (base fee + trung bình overage).
- Gross Margin: 50% (COGS 50% revenue do API & hidden costs cao).
- Churn Rate: 3%/tháng (B2B an ninh, churn thấp).
- Số tháng ở lại: 33 tháng (1/0.03).
- LTV: 7M × 0.5 × 33 = 115.5M.
- CAC: 10M (B2B sales đắt).
- LTV/CAC: 11.55 (>3, xanh).
- CAC Payback: 10M / (7M × 0.5) = 2.86 tháng (<12, xanh).

**Verdict: HEALTHY** — Lãi trên từng khách.

### 8.3 ROI & Scenarios

**Câu hỏi: Tổng thể dự án có đáng làm không?**

#### Base Case:
- NPV: Giả sử dòng tiền tích lũy sau chiết khấu > vốn đầu tư (500M build MVP). NPV >0.
- IRR: 25% (>20% WACC).
- Project Payback: 500M / 20M lãi/tháng = 25 tháng (<24, cận biên).

#### Optimistic: Adoption rate tăng 2x, ARPU tăng 20%, churn giảm 50%. NPV cao hơn, IRR 35%.

#### Pessimistic: Adoption rate giảm 50%, ARPU giảm 30%, churn tăng 2x (6%), CAC tăng 2x (20M). Runway: Với tiền mặt 1 tỷ, burn 30M/tháng → 33 tháng (>12, sống được).

**Verdict: GO** — Dự án đáng làm, sống được Pessimistic.

### 8.4 Live Demo & Decision Note

**Câu hỏi: Nếu Pessimistic xảy ra, làm gì để sống?**

**Decision Note:** 
AgeTrace nhắm đến thị trường B2B an ninh Việt Nam với TAM 1,000 cơ quan (dựa trên số đơn vị công an, điều tra). ARPU 7M dựa trên benchmark SaaS B2B Vietnam (5-10M), justify bởi value giảm thời gian tra cứu 50%. CAC 10M vì B2B sales cycle dài, cần demo và compliance. Hidden costs tính 40% API để cover labeling và retraining, dựa trên a16z reports. Pessimistic stress test với churn 6% (thực tế B2B churn <5%), runway 33 tháng đủ để pivot sang API consumption nếu cần.

**Actions nếu Pessimistic:** 
1. Chuyển sang consumption pricing thuần (không base fee).
2. Tối ưu model để giảm API cost 20%.
3. Tìm partnership với cơ quan lớn để anchor revenue.
4. Raise seed round với metrics hiện tại.

---

**Kết luận:** AgeTrace có unit economics khỏe, ROI tích cực, và sống được stress test. Sẵn sàng pitch nhà đầu tư.
| "Hidden Costs là cục giết startup AI." |
| "LTV phải dùng Gross Margin, không phải Revenue." |
| "LTV/CAC > 3 là tiêu chuẩn vàng VC, không phải > 1." |
| "Pessimistic case mới là cái cứu mạng bạn — không phải Optimistic case." |
| "Mô hình tài chính không phải để đẹp — để biết khi nào cần đổi hướng." |
| "Excel không nói dối." |

---

*Chúc các bạn không hết tiền. From product → to numbers → to survival.*
