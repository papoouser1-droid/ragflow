# Documentation: web/src/locales/vi.ts

## File Metadata

- **Path**: `web/src/locales/vi.ts`
- **Size**: 81500 bytes
- **Type**: .ts
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/locales/vi.ts`.

## Original Source Code

```ts
export default {
  translation: {
    common: {
      delete: 'Xóa',
      deleteModalTitle: 'Bạn có chắc chắn muốn xóa mục này?',
      ok: 'Có',
      cancel: 'Không',
      total: 'Tổng cộng',
      rename: 'Đổi tên',
      name: 'Tên',
      save: 'Lưu',
      namePlaceholder: 'Vui lòng nhập tên',
      next: 'Tiếp theo',
      create: 'Tạo mới',
      edit: 'Sửa',
      upload: 'Tải lên',
      english: 'Tiếng Anh',
      portugueseBr: 'Tiếng Bồ Đào Nha (Brazil)',
      chinese: 'Tiếng Trung giản thể',
      traditionalChinese: 'Tiếng Trung phồn thể',
      language: 'Ngôn ngữ',
      languageMessage: 'Vui lòng chọn ngôn ngữ của bạn!',
      languagePlaceholder: 'chọn ngôn ngữ của bạn',
      copy: 'Sao chép',
      copied: 'Đã sao chép',
      comingSoon: 'Sắp ra mắt',
      download: 'Tải xuống',
      close: 'Đóng',
      preview: 'Xem trước',
      move: 'Di chuyển',
      warn: 'Cảnh báo',
      action: 'Hành động',
      s: 'S',
      pleaseSelect: 'Vui lòng chọn',
      pleaseInput: 'Vui lòng nhập',
      submit: 'Gửi',
      vietnamese: 'Tiếng  việt',
      spanish: 'Tiếng Tây Ban Nha',
      japanese: 'Tiếng Nhật',
      embedIntoSite: 'Nhúng vào trang web',
      nextPage: 'Tới',
      previousPage: 'Lùi',
    },
    login: {
      login: 'Đăng nhập',
      signUp: 'Đăng ký',
      loginDescription: 'Rất vui được gặp lại bạn!',
      registerDescription: 'Rất vui được đón chào bạn!',
      emailLabel: 'Email',
      emailPlaceholder: 'Vui lòng nhập email',
      passwordLabel: 'Mật khẩu',
      passwordPlaceholder: 'Vui lòng nhập mật khẩu',
      rememberMe: 'Ghi nhớ tôi',
      signInTip: 'Chưa có tài khoản?',
      signUpTip: 'Đã có tài khoản?',
      nicknameLabel: 'Biệt danh',
      nicknamePlaceholder: 'Vui lòng nhập biệt danh',
      register: 'Tạo tài khoản',
      continue: 'Tiếp tục',
      title: 'Bắt đầu xây dựng trợ lý ảo của bạn.',
      description:
        'Đăng ký miễn phí để khám phá công nghệ RAG hàng đầu. Tạo cơ sở kiến thức và AI để trao quyền cho doanh nghiệp của bạn.',
      review: 'từ hơn 500 đánh giá',
    },
    header: {
      knowledgeBase: 'Cơ sở kiến thức',
      chat: 'Chat',
      register: 'Đăng ký',
      signin: 'Đăng nhập',
      home: 'Trang chủ',
      setting: 'Cài đặt người dùng',
      logout: 'Đăng xuất',
      fileManager: 'Quản lý tệp',
      flow: 'Agent',
      search: 'Tìm kiếm',
    },
    knowledgeList: {
      welcome: 'Chào mừng trở lại',
      description: 'Chúng ta sẽ sử dụng cơ sở kiến thức nào hôm nay?',
      createKnowledgeBase: 'Tạo cơ sở kiến thức',
      name: 'Tên',
      namePlaceholder: 'Vui lòng nhập tên!',
      doc: 'Tài liệu',
      searchKnowledgePlaceholder: 'Tìm kiếm',
      noMoreData: 'Tất cả chỉ có thế, không còn gì nữa',
    },
    knowledgeDetails: {
      dataset: 'Dữ liệu',
      testing: 'Kiểm tra truy hồi',
      files: 'Các tệp',
      configuration: 'Cấu hình',
      name: 'Tên',
      namePlaceholder: 'Vui lòng nhập tên!',
      doc: 'Tài liệu',
      datasetDescription:
        ' Câu hỏi và câu trả lời chỉ có thể được trả lời sau khi phân tích cú pháp thành công.',
      addFile: 'Thêm tệp',
      searchFiles: 'Tìm kiếm tệp của bạn',
      localFiles: 'Tệp cục bộ',
      emptyFiles: 'Tạo tệp trống',
      webCrawl: 'Web Crawl',
      chunkNumber: 'Số lượng khối',
      uploadDate: 'Ngày tải lên',
      chunkMethod: 'Phương thức khối',
      enabled: 'Bật',
      disabled: 'Tắt',
      action: 'Hành động',
      parsingStatus: 'Trạng thái phân tích cú pháp',
      parsingStatusTip:
        'Thời gian xử lý tài liệu thay đổi tùy theo nhiều yếu tố. Bật các tính năng như Knowledge Graph, RAPTOR, Trích xuất Câu hỏi Tự động hoặc Trích xuất Từ khóa Tự động sẽ làm tăng đáng kể thời gian xử lý. Nếu thanh tiến trình bị dừng, vui lòng tham khảo hai câu hỏi thường gặp sau: https://ragflow.io/docs/dev/faq#why-does-my-document-parsing-stall-at-under-one-percent.',
      processBeginAt: 'Bắt đầu xử lý lúc',
      processDuration: 'Thời gian xử lý',
      progressMsg: 'Thông báo tiến trình',
      testingDescription:
        'Thực hiện kiểm tra truy hồi để kiểm tra xem RAGFlow có thể phục hồi nội dung mong muốn cho LLM không.Vui lòng lưu ý rằng các thay đổi được thực hiện ở đây sẽ không được lưu tự động. Nếu bạn điều chỉnh các cài đặt mặc định ở đây, chẳng hạn như trọng số tương tự của từ khóa, hãy đảm bảo rằng bạn cập nhật các cài đặt liên quan theo cách đồng bộ trong cài đặt trợ lý trò chuyện hoặc cài đặt toán tử thu hồi.',
      similarityThreshold: 'Ngưỡng tương đồng',
      similarityThresholdTip:
        'RAGFlow sử dụng kết hợp giữa độ tương đồng từ khóa được trọng số và độ tương đồng vectơ cosin được trọng số hoặc kết hợp giữa độ tương đồng từ khóa được trọng số và điểm xếp hạng được tính lại trong quá trình truy hồi. Tham số này đặt ngưỡng cho sự tương đồng giữa truy vấn của người dùng và các khối. Bất kỳ khối nào có điểm tương đồng dưới ngưỡng này sẽ bị loại khỏi kết quả. Theo mặc định, ngưỡng được đặt thành 0,2. Điều đó có nghĩa là chỉ những đoạn có điểm tương đồng hỗn hợp từ 20 trở lên mới được truy xuất.',
      vectorSimilarityWeight: 'Trọng số tương đồng từ khóa',
      vectorSimilarityWeightTip:
        'Cài đặt trọng số của độ tương đồng từ khóa trong điểm tương đồng kết hợp, được sử dụng với độ tương đồng vectơ cosin hoặc với điểm xếp hạng được tính lại. Tổng của hai trọng số phải bằng 1.0.',
      testText: 'Văn bản kiểm tra',
      testTextPlaceholder: 'Nhập câu hỏi của bạn tại đây!',
      testingLabel: 'Kiểm tra',
      similarity: 'Giống nhau lai',
      termSimilarity: 'Giống nhau về thuật ngữ',
      vectorSimilarity: 'Giống nhau về vectơ',
      hits: 'Số lượt truy cập',
      view: 'Xem',
      filesSelected: 'Các tệp được chọn',
      upload: 'Tải lên',
      run: 'Phân tích cú pháp',
      runningStatus0: 'Chưa được giải quyết',
      runningStatus1: 'Parsing',
      runningStatus2: 'CANCEL',
      runningStatus3: 'SUCCESS',
      runningStatus4: 'FAIL',
      pageRanges: 'Phạm vi trang',
      pageRangesTip:
        'Phạm vi trang: Xác định các phạm vi trang cần phân tích cú pháp. Các trang không nằm trong các phạm vi này sẽ bị bỏ qua.',
      fromPlaceholder: 'từ',
      fromMessage: 'Thiếu số trang bắt đầu',
      toPlaceholder: 'đến',
      toMessage: 'Thiếu số trang kết thúc (được loại trừ)',
      layoutRecognize: 'Nhận dạng bố cục',
      layoutRecognizeTip:
        'Sử dụng các mô hình trực quan để phân tích bố cục nhằm xác định tốt hơn cấu trúc tài liệu, tìm vị trí của tiêu đề, khối văn bản, hình ảnh và bảng. Nếu không có tính năng này, chỉ có thể lấy được văn bản thuần của PDF. Để biết thêm thông tin, hãy xem https://ragflow.io/docs/dev/select_pdf_parser.',
      taskPageSize: 'Kích thước trang tác vụ',
      taskPageSizeMessage: 'Vui lòng nhập kích thước trang tác vụ của bạn!',
      taskPageSizeTip: `Nếu sử dụng nhận dạng bố cục, tệp PDF sẽ được chia thành các nhóm trang liên tiếp. Phân tích bố cục sẽ được thực hiện song song giữa các nhóm để tăng tốc độ xử lý. 'Kích thước trang tác vụ' xác định kích thước của các nhóm. Kích thước trang càng lớn, khả năng chia tách văn bản liên tục giữa các trang thành các khối khác nhau càng thấp.`,
      addPage: 'Thêm trang',
      greaterThan: 'Giá trị hiện tại phải lớn hơn!',
      greaterThanPrevious: 'Giá trị hiện tại phải lớn hơn giá trị trước đó!',
      selectFiles: 'Chọn tệp',
      changeSpecificCategory: 'Thay đổi danh mục cụ thể',
      uploadTitle: 'Nhấp hoặc kéo thả tệp vào khu vực này để tải lên',
      uploadDescription:
        'RAGFlow hỗ trợ tải lên tệp một lần hoặc theo lô. Đối với RAGFlow triển khai cục bộ: giới hạn tổng kích thước tệp cho mỗi lần tải lên là 1GB, với giới hạn tải lên theo lô là 32 tệp. Không có giới hạn về tổng số tệp trên mỗi tài khoản. Đối với demo.ragflow.io: giới hạn tổng kích thước tệp cho mỗi lần tải lên là 10MB, với mỗi tệp không vượt quá 10MB và tối đa là 128 tệp trên mỗi tài khoản.',
      chunk: 'Khối',
      bulk: 'Hàng loạt',
      cancel: 'Hủy bỏ',
      rerankModel: 'Mô hình xếp hạng lại',
      rerankPlaceholder: 'Vui lòng chọn',
      rerankTip: `Tùy chọn. Nếu để trống, RAGFlow sẽ sử dụng kết hợp giữa độ tương đồng từ khóa có trọng số và độ tương đồng cosine vector có trọng số; nếu chọn mô hình rerank, điểm rerank có trọng số sẽ thay thế độ tương đồng cosine vector có trọng số. Xin lưu ý rằng việc sử dụng mô hình rerank sẽ làm tăng đáng kể thời gian phản hồi của hệ thống. Nếu bạn muốn sử dụng mô hình rerank, hãy đảm bảo sử dụng SaaS reranker; nếu bạn muốn sử dụng mô hình rerank triển khai cục bộ, hãy khởi động RAGFlow bằng docker-compose-gpu.yml.`,
      topK: 'Top-K',
      topKTip: `Sử dụng cùng với Rerank model, thiết lập này xác định số lượng đoạn văn cần gửi đến mô hình reranking được chỉ định.`,
      delimiter: 'Dấu phân cách cho phân đoạn văn bản',
      html4excel: 'Excel sang HTML',
      html4excelTip: `Sử dụng cùng với phương pháp cắt khúc General. Khi chưa được bật, tệp bảng tính (XLSX, XLS (Excel 97-2003)) sẽ được phân tích theo dòng thành các cặp khóa-giá trị. Khi bật, tệp bảng tính sẽ được phân tích thành bảng HTML. Nếu bảng gốc vượt quá 12 dòng, hệ thống sẽ tự động chia thành nhiều bảng HTML mỗi 12 dòng. Để biết thêm thông tin, vui lòng xem https://ragflow.io/docs/dev/enable_excel2html.`,
      autoKeywords: 'Từ khóa tự động',
      autoKeywordsTip: `Tự động trích xuất N từ khóa cho mỗi khối để tăng thứ hạng của chúng trong các truy vấn chứa các từ khóa đó. Lưu ý rằng các token bổ sung sẽ được tiêu thụ bởi mô hình trò chuyện được chỉ định trong "Cài đặt mô hình hệ thống". Bạn có thể kiểm tra hoặc cập nhật các từ khóa đã thêm cho một khối từ danh sách khối. Để biết chi tiết, vui lòng xem https://ragflow.io/docs/dev/autokeyword_autoquestion.`,
      autoQuestions: 'Câu hỏi tự động',
      autoQuestionsTip: `Để tăng điểm xếp hạng, hãy trích xuất N câu hỏi cho mỗi đoạn kiến thức bằng mô hình trò chuyện được xác định trong "Cài đặt mô hình hệ thống". Lưu ý rằng việc này sẽ tiêu tốn thêm t

... [Content truncated - file is 67587 bytes] ...

sh: 'API',
      exeSQL: 'ExeSQL',
      exeSQLDescription:
        'Thành phần này truy vấn kết quả từ cơ sở dữ liệu quan hệ tương ứng thông qua các câu lệnh SQL. Hỗ trợ MySQL, PostgreSQL, MariaDB.',
      dbType: 'Loại cơ sở dữ liệu',
      database: 'Cơ sở dữ liệu',
      username: 'Tên người dùng',
      host: 'Máy chủ',
      port: 'Cổng',
      password: 'Mật khẩu',
      switch: 'Chuyển đổi',
      logicalOperator: 'Toán tử logic',
      switchOperatorOptions: {
        equal: 'bằng',
        notEqual: 'không bằng',
        gt: 'Lớn hơn',
        ge: 'Lớn hơn hoặc bằng',
        lt: 'Nhỏ hơn',
        le: 'Nhỏ hơn hoặc bằng',
        contains: 'Chứa',
        notContains: 'Không chứa',
        startWith: 'Bắt đầu bằng',
        endWith: 'Kết thúc bằng',
        empty: 'Trống',
        notEmpty: 'Không trống',
      },
      switchLogicOperatorOptions: {
        and: 'Và',
        or: 'Hoặc',
      },
      operator: 'Toán tử',
      value: 'Giá trị',
      useTemplate: 'Sử dụng mẫu này',
      wenCai: 'WenCai',
      queryType: 'Loại truy vấn',
      wenCaiDescription:
        'Thành phần này có thể được sử dụng để lấy thông tin về nhiều lĩnh vực tài chính, bao gồm nhưng không giới hạn ở cổ phiếu, quỹ, v.v...',
      wenCaiQueryTypeOptions: {
        stock: 'cổ phiếu',
        zhishu: 'chỉ số',
        fund: 'quỹ',
        hkstock: 'Cổ phiếu Hồng Kông',
        usstock: 'Thị trường chứng khoán Mỹ',
        threeboard: 'Thị trường OTC mới',
        conbond: 'Trái phiếu chuyển đổi',
        insurance: 'bảo hiểm',
        futures: 'hàng tương lai',
        lccp: 'Tài chính',
        foreign_exchange: 'Ngoại tệ',
      },
      akShare: 'AkShare',
      akShareDescription:
        'Thành phần này có thể được sử dụng để lấy thông tin tin tức cho cổ phiếu tương ứng từ trang web Eastmoney.',
      yahooFinance: 'YahooFinance',
      yahooFinanceDescription:
        'Thành phần này truy vấn thông tin về công ty dựa trên ký hiệu mã chứng khoán được cung cấp.',
      crawler: 'Trình thu thập dữ liệu',
      crawlerDescription:
        'Thành phần này có thể được sử dụng để thu thập mã nguồn HTML từ URL được chỉ định.',
      proxy: 'Proxy',
      crawlerResultOptions: {
        html: 'Html',
        markdown: 'Markdown',
        content: 'Nội dung',
      },
      extractType: 'Loại trích xuất',
      info: 'Thông tin',
      history: 'Lịch sử',
      financials: 'Tài chính',
      balanceSheet: 'Bảng cân đối kế toán',
      cashFlowStatement: 'Báo cáo lưu chuyển tiền tệ',
      jin10: 'Jin10',
      jin10Description:
        'Thành phần này có thể được sử dụng để truy cập thông tin trong lĩnh vực tài chính từ Nền tảng Mở Jin10, bao gồm tin tức nhanh, lịch, báo giá, tham khảo.',
      flashType: 'Loại flash',
      filter: 'Bộ lọc',
      contain: 'Chứa',
      calendarType: 'Loại lịch',
      calendarDatashape: 'Hình dạng dữ liệu lịch',
      symbolsDatatype: 'Kiểu dữ liệu biểu tượng',
      symbolsType: 'Loại biểu tượng',
      jin10TypeOptions: {
        flash: 'Tin tức nhanh',
        calendar: 'Lịch',
        symbols: 'Báo giá',
        news: 'tham khảo',
      },
      jin10FlashTypeOptions: {
        '1': 'Tin tức thị trường',
        '2': 'Tin tức tương lai',
        '3': 'Tin tức Mỹ-Hồng Kông',
        '4': 'Tin tức A-Share',
        '5': 'Tin tức hàng hóa & ngoại hối',
      },
      jin10CalendarTypeOptions: {
        cj: 'Lịch dữ liệu kinh tế vĩ mô',
        qh: 'Lịch tương lai',
        hk: 'Lịch thị trường chứng khoán Hồng Kông',
        us: 'Lịch thị trường chứng khoán Mỹ',
      },
      jin10CalendarDatashapeOptions: {
        data: 'Dữ liệu',
        event: 'Sự kiện',
        holiday: 'Ngày lễ',
      },
      jin10SymbolsTypeOptions: {
        GOODS: 'Báo giá hàng hóa',
        FOREX: 'Báo giá ngoại hối',
        FUTURE: 'Báo giá thị trường quốc tế',
        CRYPTO: 'Báo giá tiền điện tử',
      },
      jin10SymbolsDatatypeOptions: {
        symbols: 'Danh sách hàng hóa',
        quotes: 'Báo giá thị trường mới nhất',
      },
      concentrator: 'Bộ tập trung',
      concentratorDescription:
        'Một thành phần nhận đầu ra từ thành phần thượng nguồn và chuyển nó làm đầu vào cho các thành phần hạ lưu.',
      tuShare: 'TuShare',
      tuShareDescription:
        'Thành phần này có thể được sử dụng để lấy thông tin tin tức tài chính từ các trang web tài chính chính thống, hỗ trợ nghiên cứu ngành và định lượng.',
      tuShareSrcOptions: {
        sina: 'Sina',
        wallstreetcn: 'wallstreetcn',
        '10jqka': 'Straight flush',
        eastmoney: 'Eastmoney',
        yuncaijing: 'YUNCAIJING',
        fenghuang: 'FENGHUANG',
        jinrongjie: 'JRJ',
      },
      token: 'Token',
      src: 'Nguồn',
      startDate: 'Ngày bắt đầu',
      endDate: 'Ngày kết thúc',
      keyword: 'Từ khóa',
      note: 'Ghi chú',
      noteDescription: 'Ghi chú',
      notePlaceholder: 'Vui lòng nhập ghi chú',
      invoke: 'Gọi',
      invokeDescription:
        'Thành phần này có thể gọi hàm từ xa. Đặt đầu ra của các thành phần khác làm tham số hoặc đặt các tham số hằng để gọi các hàm từ xa.',
      url: 'Url',
      method: 'Phương pháp',
      timeout: 'Thời gian chờ',
      headers: 'Tiêu đề',
      cleanHtml: 'Làm sạch HTML',
      cleanHtmlTip:
        'Nếu phản hồi được định dạng HTML và chỉ muốn nội dung chính, hãy bật nó lên.',
      invalidUrl:
        'Phải là URL hợp lệ hoặc URL có chứa các biến theo định dạng {ten_bien} hoặc {thanh_phan@bien}',
      reference: 'Tham khảo',
      input: 'Đầu vào',
      output: 'Đầu ra',
      parameter: 'Tham số',
      howUseId: 'Cách sử dụng ID tác nhân?',
      content: 'Nội dung',
      operationResults: 'Kết quả hoạt động',
      autosaved: 'Tự động lưu',
      optional: 'Tùy chọn',
      pasteFileLink: 'Dán liên kết tệp',
      testRun: 'Chạy thử nghiệm',
      template: 'Mẫu',
      templateDescription: `Thành phần này được sử dụng để sắp chữ đầu ra của nhiều thành phần khác nhau.1. Hỗ trợ mẫu Jinja2, trước tiên chuyển đầu vào thành đối tượng và sau đó kết xuất mẫu. 2. Phương pháp ban đầu sử dụng thay thế chuỗi {parameter} cũng được giữ lại đồng thời`,
      arXivTip: `Thành phần này được sử dụng để lấy kết quả tìm kiếm từ https://arxiv.org/. Thông thường, nó hoạt động như một phần bổ sung cho cơ sở tri thức. Top N chỉ định số lượng kết quả tìm kiếm bạn cần điều chỉnh.`,
      googleTip: `Thành phần này được sử dụng để lấy kết quả tìm kiếm từ https://www.google.com/. Thông thường, nó hoạt động như một phần bổ sung cho cơ sở tri thức. Top N và khóa API SerpApi chỉ định số lượng kết quả tìm kiếm bạn cần điều chỉnh.`,
      bingTip: `Thành phần này được sử dụng để lấy kết quả tìm kiếm từ https://www.bing.com/. Thông thường, nó hoạt động như một phần bổ sung cho cơ sở tri thức. Top N và khóa đăng ký Bing chỉ định số lượng kết quả tìm kiếm bạn cần điều chỉnh.`,
      gitHubDescription: `Thành phần này được sử dụng để tìm kiếm các kho lưu trữ từ https://github.com/. Top N chỉ định số lượng kết quả tìm kiếm cần điều chỉnh.`,
      flow: `Quy trình làm việc`,
      emailDescription: 'Gửi email đến địa chỉ đã chỉ định',
      toEmail: 'Email người nhận',
      smtpServerRequired: 'Vui lòng nhập địa chỉ máy chủ SMTP',
      emailContent: 'Nội dung',
      smtpServer: 'SMTP Server',
      smtpPort: 'SMTP Port',
      senderEmailRequired: 'Vui lòng nhập email người gửi',
      authCodeRequired: 'Vui lòng nhập mã xác thực',
      toEmailRequired: 'Vui lòng nhập email người nhận',
      emailContentRequired: 'Vui lòng nhập nội dung email',
      emailSentSuccess: 'Email đã được gửi thành công',
      emailSentFailed: 'Không gửi được email',
      jsonFormatTip:
        'Thành phần thượng nguồn phải cung cấp chuỗi JSON theo định dạng sau:',
      emailComponent: 'Email',
      senderEmail: 'Người gửi Email',
      authCode: 'Mã xác minh',
      senderName: 'Tên người gửi',
      jsonUploadContentErrorMessage: 'lỗi tệp json',
      contentTip: 'content: Nội dung email (Tùy chọn)',
      subjectTip: 'subject: Tiêu đề email (Tùy chọn)',
      jsonUploadTypeErrorMessage: 'Vui lòng tải lên tệp json',
      dynamicParameters: 'Tham số động',
      emailSubject: 'Tiêu đề email',
      ccEmail: 'Email CC',
      toEmailTip: 'to_email: Email người nhận (Bắt buộc)',
      ccEmailTip: 'cc_email: Email CC (Tùy chọn)',
      iteration: 'Khối lặp',
      iterationDescription: `Thành phần này trước tiên chia đầu vào thành mảng bằng "dấu phân cách". Thực hiện các bước thao tác tương tự trên các phần tử trong mảng theo trình tự cho đến khi tất cả các kết quả được xuất ra, có thể được hiểu là bộ xử lý hàng loạt tác vụ. Ví dụ: trong nút lặp lại bản dịch văn bản dài, nếu tất cả nội dung được nhập vào nút LLM, có thể đạt đến giới hạn hội thoại duy nhất. Trước tiên, nút ngược dòng có thể chia văn bản dài thành nhiều mảnh và hợp tác với nút lặp đi lặp lại để thực hiện dịch hàng loạt trên mỗi phân đoạn để tránh đạt đến giới hạn tin nhắn LLM cho một cuộc hội thoại.`,
      delimiterTip: `Dấu phân cách này được sử dụng để chia văn bản đầu vào thành nhiều đoạn văn bản, tiếng vang sẽ được thực hiện dưới dạng mục đầu vào của mỗi lần lặp.`,
      delimiterOptions: {
        comma: 'Dấu phẩy',
        lineBreak: 'Ngắt dòng',
        tab: 'Tab',
        underline: 'Gạch chân',
        diagonal: 'Forward slash',
        minus: 'Dash',
        semicolon: 'Semicolon',
      },
      prompt: 'Nhắc nhở',
      promptTip:
        'Sử dụng lời nhắc hệ thống để mô tả nhiệm vụ cho LLM, chỉ định cách nó nên phản hồi và phác thảo các yêu cầu khác nhau. Lời nhắc hệ thống thường được sử dụng kết hợp với các khóa (biến), đóng vai trò là các đầu vào dữ liệu khác nhau cho LLM. Sử dụng dấu gạch chéo `/` hoặc nút (x) để hiển thị các khóa cần sử dụng.',
      promptMessage: 'Nhắc nhở là bắt buộc',
      runningHintText: 'đang chạy...🕞',
    },
    footer: {
      profile: 'All rights reserved @ React',
    },
    layout: {
      file: 'tệp',
      knowledge: 'kiến thức',
      chat: 'trò chuyện',
    },
  },
};

```

## Detailed Analysis

### File Role in Repository

The file `web/src/locales/vi.ts` is located in the `web/src/locales` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to locales.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [config.ts](config.ts_docs.md)
- [de.ts](de.ts_docs.md)
- [en.ts](en.ts_docs.md)
- [es.ts](es.ts_docs.md)
- [fr.ts](fr.ts_docs.md)
- [id.ts](id.ts_docs.md)
- [ja.ts](ja.ts_docs.md)
- [pt-br.ts](pt-br.ts_docs.md)
- [ru.ts](ru.ts_docs.md)
- [until.ts](until.ts_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
