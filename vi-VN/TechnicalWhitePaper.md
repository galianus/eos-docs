# Sách trắng kỹ thuật EOS.IO

**26 tháng 6 năm 2017**

**Tóm tắt:** EOS. Phần mềm IO giới thiệu một kiến trúc blockchain mới được thiết kế để cho phép các ứng dụng phi tập trung mở rộng theo chiều dọc và chiều ngang. Điều này đạt được bằng cách tạo ra một hệ điều hành mà ứng dụng có thể được xây dựng trên đó. Phần mềm cung cấp các tài khoản, xác thực, cơ sở dữ liệu, liên lạc không đồng bộ và lịch trình của các ứng dụng trên hàng trăm các lõi CPU hoặc cụm nhỏ. Kết quả của công nghệ này là một kiến trúc blockchain mở rộng quy mô lên đến hàng triệu giao dịch mỗi giây, giúp loại bỏ chi phí người dùng, và cho phép cho việc triển khai nhanh chóng và dễ dàng các ứng dụng không tập trung.

**XIN LƯU Ý: TOKEN MÃ HÓA ĐỀ CẬP TRONG GIẤY TRẮNG NÀY LÀ TOKEN GỐC CỦA BLOCKCHAIN PHÁT TRIỂN TRÊN PHẦN MỀM EOS. NÓ KHÔNG HỀ CHỈ ĐẾN CÁC TOKEN ERC-20 CỦA ETHEREUM LIÊN QUAN ĐẾN VIỆC PHÂN PHỐI TOKEN EOS.**

Bản quyền © block.one năm 2017

Bất kì ai cũng có thể dùng, chỉnh sửa, phát hành tất cả các nội dung trong white paper này với mục đích phi thương mại và giáo dục mà không cần phải xin phép (đối với mục đích thương mại chúng tôi sẽ áp dụng phí). Nhưng lưu ý phải đề cập đến nguồn khi sử dụng tài liệu này.

**PHỦ NHẬN:** Sách trắng này chỉ dành cho mục đích thông tin. block.one không đảm bảo tính chính xác hay kết luận đạt được trong sách trắng này, và sách trắng này được cung cấp "như hiện trạng". block.one từ chối tất cả trách nhiệm liên quan đến: (i) thị trường, mục đích, tính phù hợp, cách sử dụng,... (ii) tất cả nội dung của white paper này không chịu trách nhiệm về lỗi phát sinh; (iii) những nội dung này sẽ không vi phạm bên thứ ba. block.one và đối tác sẽ không có trách nhiệm pháp ly cho thiệt hại từ bất kỳ lý do gì. Trong bất cứ hoàn cảnh nào block.one và các bên trang liên quan không chịu trách nhiệm cho bất cứ cá nhân hoặc tổ chức nào về mọi tổn hại, mất mát, trách nhiệm pháp lý, chi phí hoặc tổn phí trực tiếp hay gián tiếp mang tính hậu quả, đền bù, ngẫu nhiên, sự thật, hình mẫu, trừng phạt hoặc đặc biệt cho sự áp dụng, trích dẫn tới, hoặc phụ thuộc vào sách trắng này hoặc bất cứ nội dung nào chứa đựng bên trong, kể cả, với không giới hạn, bất cứ tổn thất kinh doanh, lợi nhuận, doanh thu, dữ liệu, sử dụng, tình nguyện hoặc bất cứ mất mát tài sản vô hình khác.

- [Bối cảnh](#background)
- [Những yêu cầu đối với ứng dụng trên blockchain](#requirements-for-blockchain-applications) 
  - [Hỗ trợ hàng triệu người dùng](#support-millions-of-users)
  - [Sử dụng miễn phí](#free-usage)
  - [Dễ dàng nâng cấp và phục hồi lỗi](#easy-upgrades-and-bug-recovery)
  - [Độ trễ thấp](#low-latency)
  - [Tiến trình chuỗi](#sequential-performance)
  - [Tiến trình song song](#parallel-performance)
- [Thuật toán đồng thuận (DPOS)](#consensus-algorithm-dpos) 
  - [Xác nhận giao dịch](#transaction-confirmation)
  - [Giao dịch với Proof of Stake (TaPoS)](#transaction-as-proof-of-stake-tapos)
- [Tài khoản](#accounts) 
  - [Tin nhắn & xử lý](#messages--handlers)
  - [Vai trò dựa trên quản lý việc cấp phép](#role-based-permission-management) 
    - [Các mức độ cấp phép theo tên](#named-permission-levels)
    - [Tin nhắn xử lý nhóm theo tên](#named-message-handler-groups)
    - [Bản đồ cấp phép](#permission-mapping)
    - [Đánh giá cấp phép](#evaluating-permissions) 
      - [Mặc định cấp phép nhóm](#default-permission-groups)
      - [Đánh giá cấp quyền song song](#parallel-evaluation-of-permissions)
  - [Tin nhắn với lệnh trễ](#messages-with-mandatory-delay)
  - [Phục hồi khóa bị mất](#recovery-from-stolen-keys)
- [Xử lý ứng dụng song song](#deterministic-parallel-execution-of-applications) 
  - [Giảm thiểu độ trễ giao tiếp](#minimizing-communication-latency)
  - [Bộ xử lý tin nhắn chỉ-đọc](#read-only-message-handlers)
  - [Các giao dịch nguyên tử với nhiều tài khoản](#atomic-transactions-with-multiple-accounts)
  - [Đánh giá song song blockchain](#partial-evaluation-of-blockchain-state)
  - [Lịch trình tốt nhất](#subjective-best-effort-scheduling)
- [Mô hình token và sử dụng tài nguyên](#token-model-and-resource-usage) 
  - [Đo lường khách quan và chủ quan](#objective-and-subjective-measurements)
  - [Nhận Pays](#receiver-pays)
  - [Công suất đại diện](#delegating-capacity)
  - [Tách phí giao dịch từ giá trị token](#separating-transaction-costs-from-token-value)
  - [Giá lưu trữ thông tin](#state-storage-costs)
  - [Phần thưởng khối](#block-rewards)
  - [Các ứng dụng lợi ích cộng đồng](#community-benefit-applications)
- [Quản trị](#governance) 
  - [Đóng băng tài khoản](#freezing-accounts)
  - [Thay đổi mã tài khoản](#changing-account-code)
  - [Hiến pháp](#constitution)
  - [Nâng cấp giao thức & Hiến pháp](#upgrading-the-protocol--constitution) 
    - [Thay đổi khẩn cấp](#emergency-changes)
- [Lập trình scripts & Máy ảo](#scripts--virtual-machines) 
  - [Lược đồ xác định thông báo](#schema-defined-messages)
  - [Lược đồ xác định cơ sở dữ liệu](#schema-defined-database)
  - [Xác thực riêng lẽ từ ứng dụng](#separating-authentication-from-application)
  - [Kiến trúc máy ảo độc lập](#virtual-machine-independent-architecture) 
    - [Web Assembly (WASM)](#web-assembly-wasm)
    - [Máy ảo Ethereum (EVM)](#ethereum-virtual-machine-evm)
- [Giao tiếp xuyên blockchain](#inter-blockchain-communication) 
  - [Chứng minh Merkle cho việc xác thực Light Client (LCV)](#merkle-proofs-for-light-client-validation-lcv)
  - [Độ trễ của giao tiếp xuyên blockchain](#latency-of-interchain-communication)
  - [Chứng minh hoàn chỉnh](#proof-of-completeness)
- [Kết luận](#conclusion)

# Bối cảnh

Công nghệ blockchain đã được giới thiệu vào năm 2008 với sự phát hành của đồng tiền bitcoin, và kể từ đó, các nhà khởi nghiệp và lập trình viên đã và đang thử sức để khái quát hoá công nghệ này để hỗ trợ cho các ứng dụng đa dạng hơn trên một nền tảng blockchain duy nhất.

Trong khi đã có một số nền tảng blockchain đã phải vật lộn để hỗ trợ ứng dụng phân cấp chạy được, các ứng dụng blockchain cụ thể như là thị trường trao đổi phân cấp BitShares (2014) và nền tảng ứng dụng xã hội Steem (2016) đã trở thành các nền tảng blockchain nặng ký với hàng chục ngàn người dùng thường xuyên hàng ngày. Họ đã đạt được điều này bằng cách tăng hiệu năng lên hàng ngàn giao dịch trong một giây, giảm độ trễ xuống 1.5 giây, xoá bỏ phí giao dịch, và cung cấp trải nghiệm người dùng tương tự như các dịch vụ tập trung hiện tại.

Các nền tảng blockchain hiện tại đang phải cõng các chi phí khổng lồ và khả năng tính toán có giới hạn mà chính các điều đó đã ngăn cản sự phổ biến rộng rãi của công nghệ blockchain.

# Các Yêu Cầu cho Ứng Dụng trên blockchain

Để gia tăng sử dụng rộng rãi, các ứng dụng blockchain yêu cầu một nền tảng phải linh hoạt đủ để đáp ứng các yêu cầu như sau:

## Hỗ Trợ Hàng Triệu Người Dùng

Các ứng dụng đột phá như Ebay, Uber, AirBnB, và Facebook cần công nghệ blockchain với khả năng xử lý hàng chục triệu người dùng hoạt động hàng ngày. Trong một vài trường hợp, các ứng dụng có thể không hoạt động trừ khi đạt tới một lượng lớn người dùng đột biến và do đó nền tảng có thể hỗ trợ lượng lớn người dùng là tối quan trọng.

## Dùng Miễn Phí

Các lập trình viên cần sự linh hoạt để cung cấp cho người dùng các dịch vụ miễn phí; người dùng không nên trả tiền để sử dụng nền tảng hoặc hưởng lợi từ các dịch vụ của nó. Một nền tảng blockchain miễn phí cho người dùng sẽ có khả năng được đón nhận rộng rãi hơn. Lập trình viên và doanh nghiệp sau đó có thể tạo ra các chiến lược tiền tệ hiệu quả hơn.

## Dễ Nâng Cấp và Phục Hồi Lỗi

Doanh nghiệp phát triển ứng dụng chạy trên blockchain cần sự linh hoạt để tăng cường ứng dụng với các tính năng mới.

Tất cả phần mềm nghiêm túc đều bị lỗi, mặc dù với quy trình xác minh chuẩn mực khắc khe nhất. Nền tảng phải đủ mạnh mẽ để vá lỗi khi chúng xảy ra.

## Độ Trễ Thấp

Trải nghiệp người dùng tốt yêu cầu phản hồi đáng tin cậy với đỗ trễ không nhiều hơn một vài giây. Độ trễ dài hơn sẽ làm người dùng bực bội và sẽ làm cho ứng dụng xây dựng trên blockchain cạnh tranh yếu hơn so với các lựa chọn không chạy trên blockchain.

## Thi Hành Tuần Tự

Có nhiều ứng dụng đơn giản là không thể được thực thi với các giải thuật song song vì các bước phụ thuộc tuần tự vào nhau. Ứng dụng như là sàn giao dịch cần có đủ hiệu suất xử lý trình tự để giải quyết lượng lớn giao dịch và do đó, cần một nền tảng thao tác các phép tính tuần tự nhanh chóng.

## Thi Hành Song Song

Các ứng dụng quy mô lớn cần phải chia lượng công việc ra nhiều CPU hoặc là máy tính.

# Thuật toán đồng thuận (DPOS)

Nền tảng EOS.IO chỉ dùng thuật toán đồng thuận phân cấp có khả năng đáp ứng yêu cầu về hiệu suất của các ứng dụng chạy trên blockchain, [Minh Chứng Sở Hữu Được Biểu Quyết (DPOS)](https://steemit.com/dpos/@dantheman/dpos-consensus-algorithm-this-missing-white-paper). Dưới thuật toán này, những ai giữ tokens trên một blockchain sử dụng nền tảng EOS.IO có thể chọn nhóm tạo khối thông qua một hệ thống biểu quyết liên tục và bất cứ ai cũng có thể tự tham gia vào việc tạo khối và sẽ được trao quyền tao khối tỉ lệ theo tổng số lá phiếu họ đã nhận tương ứng với toàn bộ các cá nhân tạo khối khác. Trên blockchain cá nhân, người quản lý có thể sử dụng token để thêm hay loại bỏ nhân viên IT.

Nền tảng EOS.IO cho phép một khối mới được tạo ra chính xác mỗi 3 giây và chỉ duy nhất một cá nhân được cấp phép tạo khối ở một thời điểm xác định. Nếu khối không được tạo theo đúng lịch trình thì khối tương ứng với thời điểm đó sẽ bị bỏ qua. Khi một hay nhiều khối bị bỏ qua, sẽ có khoảng trống 6 giây hoặc hơn giữa blockchain.

Sử dụng nền tảng EOS.IO, khối được tạo ra theo chu kỳ 21 khối. Lúc bắt đầu mỗi chu kì, duy nhất 21 cá nhân tạo khối sẽ được chọn. 20 cá nhân có lượt bầu chọn cao nhất sẽ được tự động chọn trong mỗi chu kỳ và cá nhân cuối cùng sẽ được chọn tuỳ theo tỉ lệ số lá phiếu tương ứng với các cá nhân khác. Những cá nhân được chọn được xáo trộn sử dụng một số ngẫu nhiên trích suất từ thời gian của khối. Việc xáo trộn này được thực thi để đảm bảo rằng các cá nhân duy trì kết nối cân bằng với các cá nhân khác.

Nếu một cá nhân bỏ lỡ một khối và không tạo bất kì khối nào trong vòng 24 giờ, họ sẽ bị loại khỏi danh sách cho đến khi họ thông báo cho blockchain rằng họ muốn bắt đầu tạo khối trở lại. Việc này đảm bảo mạng lưới vận hành trơn tru bằng cách giảm thiểu số lượng khối hao hụt bởi không phải lên kế hoạch tạo khối cho các cá nhân đã được xem là không đáng tin cậy.

Trong điều kiện thông thường, blockchain sử dụng DPOS không thực hiện bất kì sự phân nhánh nào bởi vì những người tạo khối sẽ hợp tác với nhau để tạo ra khổi thay vì tranh giành nhau. Thậm chí trong trường hợp phân nhánh, sự đồng thuận sẽ tự động chuyển sang chuỗi dài nhất. Nguyên tắc hữu ích vì tốc độ khối được thêm vào sự tách nhánh blockchain trực tiếp tỉ lệ thuận với số phần trăm của cá nhân tạo khối chia sẽ cùng sự đồng thuận. Nói cách khác, phân nhánh blockchain với nhiều cá nhân tạo khối trên nó sẽ phát triển nhanh hơn so với blockchain chỉ có một hoặc vài cá nhân. Hơn nữa, cá nhân không nên tạo khối trên hai nhánh cùng một lúc. Nếu cá nhân nào vi phạm điều lệ này, họ sẽ có khả năng bị khai trừ cao. Bằng chứng mật mã của việc sản xuất khối nước đôi này cũng có thể dùng để tự động khai trừ cá nhân lạm dụng.

## Xác Nhận Giao Dịch

DPOS điển hình của blockchain có 100% nhà tạo khối tham gia. Một giao dịch có thể được xem là đã xác nhận với 99.9% chắc chắn sau trung bình 1.5 giây kể từ thời gian nó phát tín hiệu đi.

Có một vài trường hợp đặc biệt hiếm khi mà một lỗi phần mềm, nghẽn Internet, hoặc là một nhà tạo khối độc hại sẽ tạo ra hai hoặc nhiều phân nhánh. Để hoàn toàn chắc chắn rằng một giao dịch không thể bị đảo ngược, một nốt có thể chọn để chờ xác nhận từ 15 trên tổng số 21 nhà tạo khối. Dựa trên cấu hình tiêu biểu của EOS.IO, điều này có thể mất trung bình 45 giây trong điều kiện bình thường. Theo mặc định, tất cả các nốt sẽ nhìn nhận một khối đã được xác nhận bởi 15 trên 21 nhà tạo khổi là không thể đảo ngược được và sẽ không nhảy sang một nhánh không chứa khối đó bất luận chiều dài như thế nào đi nữa.

Có khả năng một nốt sẽ cảnh báo người dùng rằng có xác suất cao là nó đang ở trên một nhánh phụ trong vòng 9 giây kể từ thời gian bắt đầu rẽ nhánh. Sau 2 khối bị hụt liên tục, có khả năng 95% một nốt đang ở trên một nhánh phụ. Với 3 khối hụt liên tiếp, xác suất là 99% nốt đó đang ở trên một nhánh phụ. Việc tạo ra một mô hình tiên đoán mạnh mẽ là có khả năng, mà ở đó mô hình sẽ tận dụng thông tin về nối nào bị hụt, tỉ lệ tham gia gần đây nhất, và các yếu tố khác để nhanh chóng cảnh báo nhà vận hành rằng có điều gì đó không bình thường.

Phản ứng với cảnh báo như thế phụ thuộc hoàn toàn vào bản chất của giao dịch, nhưng phản ứng đơn giản nhất là đợi 15/31 xác nhận cho đến khi ngưng cảnh báo.

## Giao Dịch Như Là Minh Chứng Sở Hữu (TaPoS)

Phần mềm EOS.IO yêu cầu mỗi giao dịch phải có kèm theo hash của đầu khối mới nhất. Mã hash này phục vụ hai mục đích:

1. ngăn ngừa sự phát lại của một giao dịch trên các nhánh mà không kèm theo khối được tham khảo; và
2. phát tín hiệu tới mạng lưới rằng một người dùng nào đó và sở hữu của họ đang ở trên một nhánh cụ thể nào đó.

Over time all users end up directly confirming the blockchain which makes it difficult to forge counterfeit chains as the counterfeit would not be able to migrate transactions from the legitimate chain.

# Tài Khoản

EOS.IO cho phép tất cả các tài khoản được tham chiếu bởi một chuỗi từ 2 đến 32 ký tự độc nhất mà con người có thể hiểu được. Tên tài khoản này được chọn bởi người tạo tài khoản. Tất cả tài khoản phải có số dư tối thiểu vào thời điểm mà nó được tạo ra nhằm để trang trải chi phí lưu trữ dữ liệu tài khoản. Tên tài khoản cũng hỗ trợ các không gian tên mà ở đó người sở hữ của tài khoản @domain là người duy nhất có thể tạo tài khoản @user.domain.

Dưới ngữ cảnh phân cấp, nhà phát triển ứng dụng sẽ trả một lệ phí tượng trưng cho việc tạo tài khoản để đăng ký một người dùng mới. Các dịch vụ truyền thống đã trả một lượng lớn chi phí trên mỗi người dùng họ thu nạp được dưới hình thức quảng cáo, dịch vụ miễn phí. Chi phí hỗ trợ cho tài khoản mới trên blockchain sẽ không đáng kể nếu so sánh với các dịch vụ đó. May mắn là không cần phải tạo tài khoản cho người dùng đã được đăng ký bởi một ứng dụng khác.

## Tín hiệu & Xử lý

Mỗi tài khoản có thể gửi nhiều tin nhắn có cấu trúc tới các tài khoản khác và có thể định nghĩa mã lệnh để xử lý các tin nhắn nhận được. EOS.IO tạo cho mỗi tài khoản một cơ sở dữ liệu riêng mà ở đó dữ liệu chỉ có thể được truy cập bởi mã xử lý tin nhắn của riêng nó. Mã xử lý tin nhắn cũng có thể gửi tin nhắn tới các tài khoản khác. Sự tổng hợp giữa tin nhắn và mã tạo tin nhắn tự động là cách mà EOS.IO định nghĩa hợp đồng thông minh (smart contract).

## Quản Lý Cấp Phép Dựa Trên Vai Trò

Việc quản lý cấp phép có dính dáng với việc xác định một tin nhắn đã được xác nhận đúng hay chưa. Dạng cấp phép đơn giản nhất là kiểm tra rằng một tài khoản đã nhận được một chữ ký theo yêu cầu, nhưng điều này cũng đồng nghĩa rằng chữ ký theo yêu cầu đã được biết. Nhìn chung, quyền lực được cấp cho các cá nhân hoặc các nhóm của các cá nhân và thường được phân cấp. EOS.IO cung cấp hệ thống quản lý cấp phép mang tính khởi tạo mà có thể cấp cho các tài khoản quyền điều khiển cao cấp và chính xác trên việc ai được làm gì và khi nào.

Điều cực kỳ quan trọng là việc xác minh và quản lý cấp phép được chuẩn hoá và tách biệt khỏi bản chất hoạt động của ứng dụng. Điều này giúp cho việc phát triển công cụ quản lý cấp phép theo định hướng tổng quát và cũng là để cung cấp cơ hội tốt để tối ưu hoá hiệu năng.

Mỗi tài khoản có thể được quản lý bởi bất cứ kết hợp được gán gái trị của các tài khoản và mã khoá riêng tư khác. Chính điều này tạo ra cấu trúc quyền lực phân tầng mà ở đó phản ánh cách mà sự cấp phép được quản lý trong thực tế, và làm cho sự điều khiển đa người dùng với ngân quỹ dễ dàng hơn bao giờ hết. Điều khiển đa người dùng là một nhân đơn lớn nhất góp phần lớn nhất vào bảo mật, và khi được sử dụng đúng cách, nó có thể loại bỏ phần lớn nguy cơ bị mất trộm do bẻ khoá.

EOS.IO cho phép các tài khoản định nghĩa kết hợp nào của khoá và/hoặc tài khoản có thể gửi một loại tin nhắn cụ thể tới một tài khoản khác. Ví dụ, có thể có việc dùng một khoá cho một tài khoản mạng xã hội và một cái khác để dành cho việc trao đổi. Thậm chí là cho phép các tài khoản khác thay mặt chủ tài khoản để thao tác mà không cần cấp khoá cho các tài khoản đó.

### Các Mức Độ Cấp Phép Được Đặt Tên

<img align="right" src="http://eos.io/wpimg/diagram3.png" width="228.395px" height="300px" />

Dùng EOS.IO, các tài khoản có thể định nghĩa các mức độ cấp phép mà mỗi cấp độ có thể được suy ra từ mức độ cao hơn nó. Mỗi mức độ cấp phép được đặt tên xác định một thẩm quyền; một thẩm quyền là một ngưỡng kiểm tra đa chữ ký bao hàm các khoá và/hoặc các mức độ cho phép được đặt tên của các tài khoản khác. Ví dụ, một tài khoản với mức độ cấp phép "Bạn Bè" có thể được gán cho tài khoản được điều khiển ngang hàng bởi bất cứ tài khoản bạn bè khác.

Một ví dụ khác là blockchain Steem mà ở đó có 3 cấp độ cấp phép được đặt tên: chủ sở hữu, chủ động, và phát tin. Cấp độ "phát tin" có thể chỉ thực hiện các hoạt động cộng đồng như là bầu cử và phát tin, trong khi "chủ động" có thể làm mọi thứ trừ việc thay đổi người chủ sở hữu. Cấp độ "chủ sở hữu" được định nghĩa là bộ lưu trữ nguội và có thể làm mọi thứ. EOS.IO khái quát hoá khái niệm này bằng cách cho phép mỗi chủ tài khoản được định nghĩa hệ thống thứ bậc cũng như là gộp nhóm các hành động.

### Các Nhóm Xử Lý Tin Nhắn Được Đặt Tên

EOS.IO cho phép mỗi tài khoản sắp xếp bộ xử lý tin nhắn thành các nhóm được đặt tên và lồng vào nhau. Các bộ xử lý tin nhắn được đặt tên này có thể được tham chiếu bởi các tài khoản khác khi chúng cấu hình các cấp độ cấp phép.

Nhóm xử lý tin nhắn ở cấp độ cao nhất là tên tài khoản và cấp độ thấp nhất là tin nhắn đơn lẻ được nhận bởi tài khoản. Các nhóm này có thể được tham chiếu như sau: **@tên_tài_khoản.nhóm_a.nhóm_nhỏ_b.Loại_Tin_Nhắn**.

Dưới mô hình này, hợp đồng giao dịch có thể nhóm việc tạo lệnh và huỷ lệnh tách bạch khỏi gửi và rút. Việc góp nhóm bằng hợp đồng giao dịch là một tiện lợi cho các người dùng của sàn giao dịch.

### Ánh Xạ Cấp Phép

EOS.IO cho phép mỗi tài khoản định nghĩa ánh xạ giữa Nhóm Xử Lý Tin Nhắn của bất cứ tài khoản nào và Cấp Độ Cấp Phép Được Đặt Tên của chúng. Ví dụ, một chủ tài khoản có thể tạo ánh xạ của chủ tài khoản trên ứng dụng trên mạng xã hội đến nhóm cấp phép tên là "Bạn bè" của chủ tài khoản. Với ánh xạ này, bất cứ bạn bè nào cũng có thể phát tin như là chủ tài khoản trên tài khoản mạng xã hội. Mặc dù họ sẽ phát tin như là chủ tài khoản, họ sẽ vẫn có thể dùng mã khoá của bản thân để xác thực cho tin nhắn. Điều này có nghĩa là sẽ luôn luôn có thể xác minh người bạn bè nào đó đã dùng tài khoản và dùng bằng cách nào.

### Đánh Giá Cấp Phép

Khi truyền tin nhắn của loại "**Hành Động**", từ **@alice** đến **@bob** phần mềm EOS.IO sẽ trước tiên kiểm tra xem **@alice** đã định nghĩa ánh xạ cấp phép cho **@bob.groupa.subgroup.Action** hay chưa. Nếu không tìm thấy, ánh xạ cho **@bob.groupa.subgroup** sau đó là **@bob.groupa**, và cuối cùng là **@bob** sẽ được kiểm tra. Nếu không có kết quả nào khớp, ánh xạ mặc định sẽ là nhóm cấp phép được đặt tên **@alice.active**.

Một khi ánh xạ đã được xác định thì việc ký tên xác minh được được phê duyệt dùng quy trình đa-chữ-ký có giới hạn và thẩm quyền gắn với cấp phép được đặt tên. Nếu việc đó thất bại, nó sẽ tra ngược lên cấp phép của phân tử cấp trên và cao nhất là đến tận cấp phép của người sở hữu, **@alice.owner**.

<img align="center" src="http://eos.io/wpimg/diagram2grayscale2.jpg" width="845.85px" height="500px" />

#### Default Permission Groups

Công nghệ của EOS.IO cũng cho phép tất cả các tài khoản có được một "người sở hữu" của nhóm mà cá nhân đó có thể làm đượuc mọi thứ, và một nhóm "chủ động" mà ở đó có thể làm mọi thứ ngoại trừ việc thay đổi chủ nhóm. Tất cả các nhóm cấp phép khác được tạo ra dựa trên nhóm "chủ động".

#### Xác Minh Cấp Phép Song Song

Quy trình xác minh cấp phép là "chỉ đọc" và thay đổi cấp phép tạo ra bởi các giao dịch sẽ không có hiệu lực cho tới cuối khối. Điều này có nghĩa là tất cả các khoá và việc xác minh cấp phép cho tất cả các giao dịch có thể thực hiện song song. Furthermore, this means that a rapid validation of permission is possible without starting the costly application logic that would have to be rolled back. Lastly, it means that transaction permissions can be evaluated as pending transactions are received and do not need to be re-evaluated as they are applied.

All things considered, permission verification represents a significant percentage of the computation required to validate transactions. Making this a read-only and trivially parallelizable process enables a dramatic increase in performance.

When replaying the blockchain to regenerate the deterministic state from the log of messages there is no need to evaluate the permissions again. The fact that a transaction is included in a known good block is sufficient to skip this step. This dramatically reduces the computational load associated with replaying an ever growing blockchain.

## Messages with Mandatory Delay

Time is a critical component of security. In most cases, it is not possible to know if a private key has been stolen until it has been used. Time based security is even more critical when people have applications that require keys be kept on computers connected to the internet for daily use. The EOS.IO software enables application developers to indicate that certain messages must wait a minimum period of time after being included in a block before they can be applied. During this time they can be cancelled.

Users can then receive notice via email or text message when one of these messages is broadcast. If they did not authorize it, then they can use the account recovery process to recover their account and retract the message.

The required delay depends upon how sensitive an operation is. Paying for a coffee can have no delay and be irreversible in seconds, while buying a house may require a 72 hour clearing period. Transferring an entire account to new control may take up to 30 days. The exact delays chosen are up to application developers and users.

## Recovery from Stolen Keys

The EOS.IO software provides users a way to restore control of their account when their keys are stolen. An account owner can use any owner key that was active in the last 30 days along with approval from their designated account recovery partner to reset the owner key on their account. The account recovery partner cannot reset control of the account without the help of the owner.

There is nothing for the hacker to gain by attempting to go through the recovery process because they already "control" the account. Furthermore, if they did go through the process, the recovery partner would likely demand identification and multi-factor authentication (phone and email). This would likely compromise the hacker or gain the hacker nothing in the process.

This process is also very different from a simple multi-signature arrangement. With a multi-signature transaction, there is another company that is party to every transaction that is executed, but with the recovery process the agent is only a party to the recovery process and has no power over the day-to-day transactions. This dramatically reduces costs and legal liabilities for everyone involved.

# Deterministic Parallel Execution of Applications

Blockchain consensus depends upon deterministic (reproducible) behavior. This means all parallel execution must be free from the use of mutexes or other locking primitives. Without locks there must be some way to guarantee that all accounts can only read and write their own private database. It also means that each account processes messages sequentially and that parallelism will be at the account level.

In an EOS.IO software-based blockchain, it is the job of the block producer to organize message delivery into independent threads so that they can be evaluated in parallel. The state of each account depends only upon the messages delivered to it. The schedule is the output of a block producer and will be deterministically executed, but the process for generating the schedule need not be deterministic. This means that block producers can utilize parallel algorithms to schedule transactions.

Part of parallel execution means that when a script generates a new message it does not get delivered immediately, instead it is scheduled to be delivered in the next cycle. The reason it cannot be delivered immediately is because the receiver may be actively modifying its own state in another thread.

## Minimizing Communication Latency

Latency is the time it takes for one account to send a message to another account and then receive a response. The goal is to enable two accounts to exchange messages back and forth within a single block without having to wait 3 seconds between each message. To enable this, the EOS.IO software divides each block into cycles. Each cycle is divided into threads and each thread contains a list of transactions. Each transaction contains a set of messages to be delivered. This structure can be visualized as a tree where alternating layers are processed sequentially and in parallel.

        Block
    
          Cycles (sequential)
    
            Threads (parallel)
    
              Transactions (sequential)
    
                Messages (sequential)
    
                  Receiver and Notified Accounts (parallel)
    

Transactions generated in one cycle can be delivered in any subsequent cycle or block. Block producers will keep adding cycles to a block until the maximum wall clock time has passed or there are no new generated transactions to deliver.

It is possible to use static analysis of a block to verify that within a given cycle no two threads contain transactions that modify the same account. So long as that invariant is maintained a block can be processed by running all threads in parallel.

## Read-Only Message Handlers

Some accounts may be able to process a message on a pass/fail basis without modifying their internal state. If this is the case then these handlers can be executed in parallel so long as only read-only message handlers for a particular account are included in one or more threads within a particular cycle.

## Atomic Transactions with Multiple Accounts

Sometimes it is desirable to ensure that messages are delivered to and accepted by multiple accounts atomically. In this case both messages are placed in one transaction and both accounts will be assigned the same thread and the messages applied sequentially. This situation is not ideal for performance and when it comes to "billing" users for usage, they will get billed by the number of unique accounts referenced by a transaction.

For performance and cost reasons it is best to minimize atomic operations involving two or more heavily utilized accounts.

## Partial Evaluation of Blockchain State

Scaling blockchain technology necessitates that components are modular. Everyone should not have to run everything, especially if they only need to use a small subset of the applications.

An exchange application developer runs full nodes for the purpose of displaying the exchange state to its users. This exchange application has no need for the state associated with social media applications. EOS.IO software allows any full node to pick any subset of applications to run. Messages delivered to other applications are safely ignored because an application's state is derived entirely from the messages that are delivered to it.

This has some significant implications on communication with other accounts. Most significantly it cannot be assumed that the state of the other account is accessible on the same machine. It also means that while it is tempting to enable "locks" that allow one account to synchronously call another account, this design pattern breaks down if the other account is not resident in memory.

All state communication among accounts must be passed via messages included in the blockchain.

## Subjective Best Effort Scheduling

The EOS.IO software cannot obligate block producers to deliver any message to any other account. Each block producer makes their own subjective measurement of the computational complexity and time required to process a transaction. This applies whether a transaction is generated by a user or automatically by a script.

On a launched blockchain adopting the EOS.IO software, at a network level all transactions are billed a fixed computational bandwidth cost regardless of whether it took .01ms or a full 10 ms to execute it. However, each individual block producer using the software may calculate resource usage using their own algorithm and measurements. When a block producer concludes that a transaction or account has consumed a disproportionate amount of the computational capacity they simply reject the transaction when producing their own block; however, they will still process the transaction if other block producers consider it valid.

In general so long as even 1 block producer considers a transaction as valid and under the resource usage limits then all other block producers will also accept it, but it may take up to 1 minute for the transaction to find that producer.

In some cases a producer may create a block that includes transactions that are an order of magnitude outside of acceptable ranges. In this case the next block producer may opt to reject the block and the tie will be broken by the third producer. This is no different than what would happen if a large block caused network propagation delays. The community would notice a pattern of abuse and eventually remove votes from the rogue producer.

This subjective evaluation of computational cost frees the blockchain from having to precisely and deterministically measure how long something takes to run. With this design there is no need to precisely count instructions which dramatically increases opportunities for optimization without breaking consensus.

# Token Model and Resource Usage

**PLEASE NOTE: CRYPTOGRAPHIC TOKENS REFERRED TO IN THIS WHITE PAPER REFER TO CRYPTOGRAPHIC TOKENS ON A LAUNCHED BLOCKCHAIN THAT ADOPTS THE EOS.IO SOFTWARE. THEY DO NOT REFER TO THE ERC-20 COMPATIBLE TOKENS BEING DISTRIBUTED ON THE ETHEREUM BLOCKCHAIN IN CONNECTION WITH THE EOS TOKEN DISTRIBUTION.**

All blockchains are resource constrained and require a system to prevent abuse. With a blockchain that uses EOS.IO software, there are three broad classes of resources that are consumed by applications:

1. Bandwidth and Log Storage (Disk);
2. Computation and Computational Backlog (CPU); and
3. State Storage (RAM).

Bandwidth and computation have two components, instantaneous usage and long-term usage. A blockchain maintains a log of all messages and this log is ultimately stored and downloaded by all full nodes. With the log of messages it is possible to reconstruct the state of all applications.

The computational debt is calculations that must be performed to regenerate state from the message log. If the computational debt grows too large then it becomes necessary to take snapshots of the blockchain's state and discard the blockchain's history. If computational debt grows too quickly then it may take 6 months to replay 1 year worth of transactions. It is critical, therefore, that the computational debt be carefully managed.

Blockchain state storage is information that is accessible from application logic. It includes information such as order books and account balances. If the state is never read by the application then it should not be stored. For example, blog post content and comments are not read by application logic so they should not be stored in the blockchain's state. Meanwhile the existence of a post/comment, the number of votes, and other properties do get stored as part of the blockchain's state.

Block producers publish their available capacity for bandwidth, computation, and state. The EOS.IO software allows each account to consume a percentage of the available capacity proportional to the amount of tokens held in a 3-day staking contract. For example, if a blockchain based on the EOS.IO software is launched and if an account holds 1% of the total tokens distributable pursuant to that blockchain, then that account has the potential to utilize 1% of the state storage capacity.

Adopting the EOS.IO software on a launched blockchain means bandwidth and computational capacity are allocated on a fractional reserve basis because they are transient (unused capacity cannot be saved for future use). The algorithm used by EOS.IO software is similar to the algorithm used by Steem to rate-limit bandwidth usage.

## Objective and Subjective Measurements

As discussed earlier, instrumenting computational usage has a significant impact on performance and optimization; therefore, all resource usage constraints are ultimately subjective and enforcement is done by block producers according to their individual algorithms and estimates.

That said, there are certain things that are trivial to measure objectively. The number of messages delivered and the size of the data stored in the internal database are cheap to measure objectively. The EOS.IO software enables block producers to apply the same algorithm over these objective measures but may choose to apply stricter subjective algorithms over subjective measurements.

## Receiver Pays

Traditionally, it is the business that pays for office space, computational power, and other costs required to run the business. The customer buys specific products from the business and the revenue from those product sales is used to cover the business costs of operation. Similarly, no website obligates its visitors to make micropayments for visiting its website to cover hosting costs. Therefore, decentralized applications should not force its customers to pay the blockchain directly for the use of the blockchain.

A launched blockchain that uses the EOS.IO software does not require its users to pay the blockchain directly for its use and therefore does not constrain or prevent a business from determining its own monetization strategy for its products.

## Delegating Capacity

A holder of tokens on a blockchain launched adopting the EOS.IO software who may not have an immediate need to consume all or part of the available bandwidth, can give or rent such unconsumed bandwidth to others; the block producers running EOS.IO software on such blockchain will recognize this delegation of capacity and allocate bandwidth accordingly.

## Separating Transaction costs from Token Value

One of the major benefits of the EOS.IO software is that the amount of bandwidth available to an application is entirely independent of any token price. If an application owner holds a relevant number of tokens on a blockchain adopting EOS.IO software, then the application can run indefinitely within a fixed state and bandwidth usage. In such case, developers and users are unaffected from any price volatility in the token market and therefore not reliant on a price feed. In other words, a blockchain that adopts the EOS.IO software enables block producers to naturally increase bandwidth, computation, and storage available per token independent of the token's value.

A blockchain using EOS.IO software also awards block producers tokens every time they produce a block. The value of the tokens will impact the amount of bandwidth, storage, and computation a producer can afford to purchase; this model naturally leverages rising token values to increase network performance.

## State Storage Costs

While bandwidth and computation can be delegated, storage of application state will require an application developer to hold tokens until that state is deleted. If state is never deleted then the tokens are effectively removed from circulation.

Every user account requires a certain amount of storage; therefore, every account must maintain a minimum balance. As storage capacity of the network increases this minimum required balance will fall.

## Block Rewards

A blockchain that adopts the EOS.IO software will award new tokens to a block producer every time a block is produced. In these circumstances, the number of tokens created is determined by the median of the desired pay published by all block producers. The EOS.IO software may be configured to enforce a cap on producer awards such that the total annual increase in token supply does not exceed 5%.

## Community Benefit Applications

In addition to electing block producers, pursuant to a blockchain based on the EOS.IO software, users can elect 3 community benefit applications also known as smart contracts. These 3 applications will receive tokens of up to a configured percent of the token supply per annum minus the tokens that have been paid to block producers. These smart contracts will receive tokens proportional to the votes each application has received from token holders. The elected applications or smart contracts can be replaced by newly elected applications or smart contracts by token holders.

# Governance

Governance is the process by which people reach consensus on subjective matters that cannot be captured entirely by software algorithms. An EOS.IO software-based blockchain implements a governance process that efficiently directs the existing influence of block producers. Absent a defined governance process, prior blockchains relied ad hoc, informal, and often controversial governance processes that result in unpredictable outcomes.

A blockchain based on the EOS.IO software recognizes that power originates with the token holders who delegate that power to the block producers. The block producers are given limited and checked authority to freeze accounts, update defective applications, and propose hard forking changes to the underlying protocol.

Embedded into the EOS.IO software is the election of block producers. Before any change can be made to the blockchain these block producers must approve it. If the block producers refuse to make changes desired by the token holders then they can be voted out. If the block producers make changes without permission of the token holders then all other non-producing full-node validators (exchanges, etc) will reject the change.

## Freezing Accounts

Sometimes a smart contact behaves in an aberrant or unpredictable manner and fails to perform as intended; other times an application or account may discover an exploit that enables it to consume an unreasonable amount of resources. When such issues inevitably occur, the block producers have the power to rectify such situations.

The block producers on all blockchains have the power to select which transactions are included in blocks which gives them the ability to freeze accounts. A blockchain using EOS.IO software formalizes this authority by subjecting the process of freezing an account to a 17/21 vote of active producers. If the producers abuse the power they can be voted out and an account will be unfrozen.

## Changing Account Code

When all else fails and an "unstoppable application" acts in an unpredictable manner, a blockchain using EOS.IO software allows the block producers to replace the account's code without hard forking the entire blockchain. Similar to the process of freezing an account, this replacement of the code requires a 17/21 vote of elected block producers.

## Constitution

The EOS.IO software enables blockchains to establish a peer-to-peer terms of service agreement or a binding contract among those users who sign it, referred to as a "constitution". The content of this constitution defines obligations among the users which cannot be entirely enforced by code and facilitates dispute resolution by establishing jurisdiction and choice of law along with other mutually accepted rules. Every transaction broadcast on the network must incorporate the hash of the constitution as part of the signature and thereby explicitly binds the signer to the contract.

The constitution also defines the human-readable intent of the source code protocol. This intent is used to identify the difference between a bug and a feature when errors occur and guides the community on what fixes are proper or improper.

## Upgrading the Protocol & Constitution

The EOS.IO software defines a process by which the protocol as defined by the canonical source code and its constitution, can be updated using the following process:

1. Block producers propose a change to the constitution and obtains 17/21 approval.
2. Block producers maintain 17/21 approval for 30 consecutive days.
3. All users are required to sign transactions using the hash of the new constitution.
4. Block producers adopt changes to the source code to reflect the change in the constitution and propose it to the blockchain using the hash of a git commit.
5. Block producers maintain 17/21 approval for 30 consecutive days.
6. Changes to the code take effect 7 days later, giving all full nodes 1 week to upgrade after ratification of the source code.
7. All nodes that do not upgrade to the new code shut down automatically.

By default configuration of the EOS.IO software, the process of updating the blockchain to add new features takes 2 to 3 months, while updates to fix non-critical bugs that do not require changes to the constitution can take 1 to 2 months.

### Emergency Changes

The block producers may accelerate the process if a software change is required to fix a harmful bug or security exploit that is actively harming users. Generally speaking it could be against the constitution for accelerated updates to introduce new features or fix harmless bugs.

# Scripts & Virtual Machines

The EOS.IO software will be first and foremost a platform for coordinating the delivery of authenticated messages to accounts. The details of scripting language and virtual machine are implementation specific details that are mostly independent from the design of the EOS.IO technology. Any language or virtual machine that is deterministic and properly sandboxed with sufficient performance can be integrated with the EOS.IO software API.

## Schema Defined Messages

All messages sent between accounts are defined by a schema which is part of the blockchain consensus state. This schema allows seamless conversion between binary and JSON representation of the messages.

## Schema Defined Database

Database state is also defined using a similar schema. This ensures that all data stored by all applications is in a format that can be interpreted as human readable JSON but stored and manipulated with the efficiency of binary.

## Separating Authentication from Application

To maximize parallelization opportunities and minimize the computational debt associated with regenerating application state from the transaction log, EOS.IO software separates validation logic into three sections:

1. Validating that a message is internally consistent;
2. Validating that all preconditions are valid; and
3. Modifying the application state.

Validating the internal consistency of a message is read-only and requires no access to blockchain state. This means that it can be performed with maximum parallelism. Validating preconditions, such as required balance, is read-only and therefore can also benefit from parallelism. Only modification of application state requires write access and must be processed sequentially for each application.

Authentication is the read-only process of verifying that a message can be applied. Application is actually doing the work. In real time both calculations are required to be performed, however once a transaction is included in the blockchain it is no longer necessary to perform the authentication operations.

## Virtual Machine Independent Architecture

It is the intention of the EOS.IO software-based blockchain that multiple virtual machines can be supported and new virtual machines added over time as necessary. For this reason, this paper will not discuss the details of any particular language or virtual machine. That said, there are two virtual machines that are currently being evaluated for use with an EOS.IO software-based blockchain.

### Web Assembly (WASM)

Web Assembly is an emerging web standard for building high performance web applications. With a few small modifications Web Assembly can be made deterministic and sandboxed. The benefit of Web Assembly is the widespread support from industry and that it enables contracts to be developed in familiar languages such as C or C++.

Ethereum developers have already begun modifying Web Assembly to provide suitable sandboxing and determinism in with their [Ethereum flavored Web Assembly (WASM)](https://github.com/ewasm/design). This approach can be easily adapted and integrated with EOS.IO software.

### Ethereum Virtual Machine (EVM)

This virtual machine has been used for most existing smart contracts and could be adapted to work within an EOS.IO blockchain. It is conceivable that EVM contracts could be run within their own sandbox inside an EOS.IO software-based blockchain and that with some adaptation EVM contracts could communicate with other EOS.IO software blockchain applications.

# Inter Blockchain Communication

EOS.IO software is designed to facilitate inter-blockchain communication. This is achieved by making it easy to generate proof of message existence and proof of message sequence. These proofs combined with an application architecture designed around message passing enables the details of inter-blockchain communication and proof validation to be hidden from application developers.

<img align="right" src="http://eos.io/wpimg/Diagram1.jpg" width="362.84px" height="500px" />

## Merkle Proofs for Light Client Validation (LCV)

Integrating with other blockchains is much easier if clients do not need to process all transactions. After all, an exchange only cares about transfers in and out of the exchange and nothing more. It would also be ideal if the exchange chain could utilize lightweight merkle proofs of deposit rather than having to trust its own block producers entirely. At the very least a chain's block producers would like to maintain the smallest possible overhead when synchronizing with another blockchain.

The goal of LCV is to enable the generation of relatively light-weight proof of existence that can be validated by anyone tracking a relatively light-weight data set. In this case the objective is to prove that a particular transaction was included in a particular block and that the block is included in the verified history of a particular blockchain.

Bitcoin supports validation of transactions assuming all nodes have access to the full history of block headers which amounts to 4MB of block headers per year. At 10 transactions per second, a valid proof requires about 512 bytes. This works well for a blockchain with a 10 minute block interval, but is no longer "light" for blockchains with a 3 second block interval.

The EOS.IO software enables lightweight proofs for anyone who has any irreversible block header after the point in which the transaction was included. Using the hash-linked structure shown below it is possible to prove the existence of any transaction with a proof less than 1024 bytes in size. If it is assumed that validating nodes are keeping up with all block headers in the past day (2 MB of data), then proving these transactions will only require proofs 200 bytes long.

There is little incremental overhead associated with producing blocks with the proper hash-linking to enable these proofs which means there is no reason not to generate blocks this way.

When it comes time to validate proofs on other chains there are a wide variety of time/ space/ bandwidth optimizations that can be made. Tracking all block headers (420 MB/year) will keep proof sizes small. Tracking only recent headers can offer a trade off between minimal long-term storage and proof size. Alternatively, a blockchain can use a lazy evaluation approach where it remembers intermediate hashes of past proofs. New proofs only have to include links to the known sparse tree. The exact approach used will necessarily depend upon the percentage of foreign blocks that include transactions referenced by merkle proof.

After a certain density of interconnectedness it becomes more efficient to simply have one chain contain the entire block history of another chain and eliminate the need for proofs all together. For performance reasons, it is ideal to minimize the frequency of inter-chain proofs.

## Latency of Interchain Communication

When communicating with another outside blockchain, block producers must wait until there is 100% certainty that a transaction has been irreversibly confirmed by the other blockchain before accepting it as a valid input. Using an EOS.IO software-based blockchain and DPOS with 3 second blocks and 21 producers, this takes approximately 45 seconds. If a chain's block producers do not wait for irreversibility it would be like an exchange accepting a deposit that was later reversed and could impact the validity of the blockchain's consensus.

## Proof of Completeness

When using merkle proofs from outside blockchains, there is a significant difference between knowing that all transactions processed are valid and knowing that no transactions have been skipped or omitted. While it is impossible to prove that all of the most recent transactions are known, it is possible to prove that there have been no gaps in the transaction history. The EOS.IO software facilitates this by assigning a sequence number to every message delivered to every account. A user can use these sequence numbers to prove that all messages intended for a particular account have been processed and that they were processed in order.

# Conclusion

The EOS.IO software is designed from experience with proven concepts and best practices, and represents fundamental advancements in blockchain technology. The software is part of a holistic blueprint for a globally scalable blockchain society in which decentralised applications can be easily deployed and governed.