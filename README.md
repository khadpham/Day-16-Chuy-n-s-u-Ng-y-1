***

# Day 16 Submission — Team Đan Kha - 2A202600253

## Members
- Phạm Đan Kha — Data Scientist / AI Product Owner

---

## 1. Idea reframed

Original idea:
> Xây dựng một hệ thống AI chatbot (RAG) đọc tài liệu quy trình để giúp nhân viên back-office tra cứu thông tin nhanh hơn.

Reframed as a product opportunity:
> Các doanh nghiệp FMCG đang lãng phí hàng tỷ đồng [VND] mỗi năm [year] do đứt gãy chuỗi cung ứng, hàng tồn kho quá hạn (expired) và dự báo nhu cầu sai lệch. Bằng cách kết hợp **RAG (tri thức quy trình)** với **Real-time Data Access (dữ liệu kho thực tế)** và **Predictive Analytics (mô hình dự báo)**, chúng ta tạo ra một "AI Logistics Commander" giúp back-office không chỉ biết "quy trình làm gì" mà còn biết "hàng đang ở đâu, bao giờ thì hết, và cần nhập bao nhiêu" một cách tự động.

---

## 2. Customer / Segment Card

- **Segment name:** Bộ phận Quản lý Kho vận & Điều phối Chuỗi cung ứng (Inventory & Supply Chain Ops) tại các doanh nghiệp FMCG vừa và nhỏ [SME].
- **Operational context:** Quản lý danh mục hàng nghìn SKU với vòng đời ngắn, phải liên tục đối soát giữa số liệu ERP và thực tế kho, đồng thời chịu áp lực tối ưu hóa vòng quay hàng tồn kho (Inventory Turnover).
- **Recurring workflow:** Kiểm tra mức tồn kho an toàn (Safety Stock); dự báo nhu cầu nhập hàng theo mùa vụ/campaign; tra cứu SOP xử lý hàng lỗi/hàng cận date và lập báo cáo batch tổng hợp tình hình kho.
- **Pain moment:** Khi hệ thống ERP báo còn hàng nhưng thực tế kho đã hết (hoặc ngược lại), dẫn đến việc từ chối đơn hàng của khách hoặc hàng bị lưu kho quá lâu gây hỏng hóc, mất trắng chi phí [cost] (chi phí).
- **Why now:** Sự sẵn sàng của các nền tảng Vector Database và khả năng tích hợp LLM với Tool-calling (Agentic Workflow) cho phép AI truy cập dữ liệu SQL/ERP thời gian thực một cách an toàn và chính xác.
- **Access path:** Triển khai module "Dự báo & Cảnh báo thông minh" tích hợp trực tiếp vào database hiện có của bộ phận kho vận để chứng minh tính chính xác trong 30 ngày [day] (ngày).

One-sentence description:
> Đội ngũ vận hành kho vận đang phải đưa ra các quyết định "triệu đô" dựa trên những báo cáo Excel rời rạc, có độ trễ và thiếu tính dự báo thông minh.

---

## 3. Need Map (3 needs)

### Need #1 (Priority) - Real-time Inventory Intelligence
- **Statement (JTBD):** Khi [phát hiện sự cố lệch tồn kho hoặc cần kiểm tra hàng gấp cho một đơn hàng lớn], tôi muốn [AI truy cập trực tiếp dữ liệu kho và tóm tắt tình trạng thực tế ngay lập tức], để tôi có thể [ra quyết định điều phối chính xác mà không cần đợi nhân viên kho kiểm đếm thủ công].
- **Current workaround:** Xuất file thô từ phần mềm quản lý kho (WMS), gọi điện/Zalo cho thủ kho xác nhận, sau đó tự tổng hợp lại trên Excel.
- **Pain signal:** Thông tin bị "vênh" giữa các bộ phận; mất cơ hội bán hàng [sales] (bán hàng) vì phản hồi khách chậm [slow] (chậm).
- **Evidence / proxy evidence:** Tỷ lệ đơn hàng bị hủy do "hết hàng ảo" (stock-out) chiếm khoảng 5-10% tổng doanh số trong các kỳ cao điểm.
- **Why underserved:** WMS hiện tại chỉ là lớp lưu trữ dữ liệu (Data Storage), không có lớp giao diện ngôn ngữ tự nhiên để truy vấn và phân tích "on-the-fly".

### Need #2 - Predictive Demand & Stock-out Prevention
- **Statement (JTBD):** Khi [chuẩn bị cho các chiến dịch Marketing hoặc mùa cao điểm], tôi muốn [hệ thống tự động dự báo lượng hàng cần thiết dựa trên dữ liệu lịch sử và xu hướng], để tôi có thể [lập kế hoạch nhập hàng tối ưu, tránh tình trạng cháy hàng hoặc tồn kho quá mức gây đọng vốn].
- **Current workaround:** Dựa vào kinh nghiệm cá nhân của quản lý lâu năm hoặc dùng các công thức dự báo đơn giản trên Excel vốn không xử lý được các biến số phức tạp.
- **Pain signal:** Hàng tồn kho cận date tăng cao; kho bãi quá tải do nhập hàng sai thời điểm.
- **Evidence / proxy evidence:** Chi phí lưu kho và tiêu hủy hàng quá hạn chiếm tỷ trọng đáng kể trong chi phí vận hành (Operating Cost).
- **Why underserved:** Các công cụ dự báo chuyên sâu thường quá đắt đỏ và khó sử dụng đối với các doanh nghiệp SME tại Việt Nam.

### Need #3 - Automated Operating SOP & Exception Handling
- **Statement (JTBD):** Khi [xảy ra các tình huống ngoại lệ như hàng hỏng, trả hàng hoặc lỗi hệ thống], tôi muốn [AI hướng dẫn quy trình xử lý chuẩn (SOP) dựa trên bối cảnh dữ liệu thực tế của lô hàng đó], để tôi có thể [xử lý sự cố đúng quy định, minh bạch và nhanh chóng].
- **Current workaround:** Lục tìm trong sổ tay quy trình hoặc chờ chỉ đạo từ cấp trên qua các group chat.
- **Pain signal:** Nhân viên mới xử lý sai quy trình gây thất thoát hàng hoá; quy trình bàn giao giữa các ca bị đứt đoạn thông tin.
- **Evidence / proxy evidence:** Thời gian xử lý một khiếu nại/sự cố kho vận kéo dài trung bình 2-3 ngày [day] (ngày) do vướng mắc quy trình.
- **Why underserved:** Tài liệu SOP hiện tại hoàn toàn tách rời với dữ liệu hàng hoá thực tế, nhân viên phải tự kết nối thông tin bằng tay.

---

## 4. Strategy Statement

For [đội ngũ quản lý kho vận và back-office của các công ty FMCG SME]
who struggle with [việc thiếu tầm nhìn dữ liệu thời gian thực và sai lệch trong dự báo nhu cầu hàng hoá],
our product helps them [tự động hoá trí tuệ vận hành, dự báo chính xác và tối ưu hoá tồn kho]
through [hệ thống AI Agent có khả năng truy cập database thực tế, kết hợp RAG quy trình và mô hình dự báo batch],
unlike [việc sử dụng các báo cáo Excel thủ công và cảm tính cá nhân],
because we can leverage [thuật toán dự báo chuyên biệt cho ngành FMCG Việt Nam và khả năng nhúng sâu vào workflow báo cáo hàng ngày].

---

## 5. Moat Hypothesis

**Moat mechanism:** **Data Compounding (Cộng dồn dữ liệu)** kết hợp với **Workflow Embedding**.

If we deploy 50 times in different FMCG warehouses, the following improve:
1. **Predictive Accuracy:** Hệ thống càng xem nhiều chu kỳ xuất-nhập hàng, các mô hình dự báo nhu cầu (Demand Forecasting) càng chính xác và khó bị sao chép bởi các model generic.
2. **Exception Logic:** AI học được hàng nghìn kịch bản lỗi vận hành thực tế tại VN, từ đó đưa ra các gợi ý xử lý SOP sắc bén hơn hẳn đối thủ mới.
3. **Integration Moat:** Khi AI đã được cấp quyền truy cập sâu vào Database lõi và trở thành nơi nhân viên làm báo cáo hàng ngày, chi phí chuyển đổi (Switching Cost) sẽ cực kỳ cao.

Why competitors cannot easily replicate this:
> Lợi thế bền vững [durable] (bền vững) đến từ lớp dữ liệu vận hành thực tế đã được "AI tiêu hoá" và tinh chỉnh cho riêng thị trường Việt Nam.

---

## 6. Initial TAM / SAM / SOM view (VND)

*(Giả định phí SaaS cho gói Premium có Predictive Analytics: 4.500.000 VNĐ/kho/tháng = 54.000.000 VNĐ/năm)*

| Layer | Estimate | Key assumptions | Confidence |
|---|---|---|---|
| TAM | ~2.160 Tỷ VNĐ/năm | 40.000 kho vận/trung tâm phân phối tiềm năng tại VN * Phí SaaS 54tr/năm. | Med |
| SAM | ~540 Tỷ VNĐ/năm | 10.000 doanh nghiệp SME FMCG/Bán lẻ hiện đại có nhu cầu dự báo và quản lý kho chuyên sâu. | Med |
| SOM | ~27 - 40 Tỷ VNĐ/năm | Mục tiêu chiếm lĩnh 500 - 750 kho trong 24 tháng đầu tiên. | High |

**Top 3 unknowns requiring further research:**
1. Khả năng kết nối kỹ thuật (API/Connector) với các hệ thống ERP đời cũ hoặc database nội bộ của SME.
2. WTP (Mức độ sẵn lòng chi trả) của các chủ doanh nghiệp cho tính năng "Dự báo" so với tính năng "Tiết kiệm nhân sự".
3. Độ tin cậy của dữ liệu đầu vào (Garbage in, Garbage out): Nếu dữ liệu kho của khách hàng quá nát, AI sẽ dự báo sai.

**Judgment:**
- [x] Worth pursuing now (đặc biệt là giá trị từ việc giảm thất thoát hàng expired và stock-out).

---

## 7. Positioning Note (2 sentences)

**What we are:**
> Một "Bộ não Dự báo và Vận hành" (Operating Intelligence) giúp biến dữ liệu kho tĩnh thành các hành động điều phối chính xác và chủ động.

**What we are not / not yet:**
> Chúng tôi không phải là một phần mềm WMS để nhập liệu, mà là lớp trí tuệ phía trên để tối ưu hoá mọi quyết định của WMS hiện có.

---

## 8. Self-assessment before Day 17

Trong 6 mắt xích (Idea → Customer → Need → Strategy → Moat → Market Size), mắt xích nào team đang yếu nhất?
> Mắt xích **Strategy**. Chúng tôi cần xác định rõ: Nên thâm nhập (Wedge) bằng tính năng "Tra cứu & Báo cáo" (ít rủi ro) hay đánh thẳng vào "Dự báo nhu cầu" (giá trị cao nhưng khó làm chuẩn ngay từ đầu)?

***
