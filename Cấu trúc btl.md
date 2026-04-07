BTL/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── auction/
│   │   │           ├── client/
│   │   │           │   ├── controller/
│   │   │           │   │   ├── AdminDashboardController.java
│   │   │           │   │   ├── AuctionDetailController.java
│   │   │           │   │   ├── AuctionListController.java
│   │   │           │   │   ├── LoginController.java
│   │   │           │   │   ├── MainController.java
│   │   │           │   │   ├── RegisterController.java
│   │   │           │   │   └── SellerDashboardController.java
│   │   │           │   ├── AuctionClient.java
│   │   │           │   └── NetworkClient.java
│   │   │           │
│   │   │           ├── common/
│   │   │           │   ├── exception/
│   │   │           │   │   ├── AuctionClosedException.java
│   │   │           │   │   ├── AuctionException.java
│   │   │           │   │   ├── AuthenticationException.java
│   │   │           │   │   └── InvalidBidException.java
│   │   │           │   └── model/
│   │   │           │       ├── Admin.java
│   │   │           │       ├── Art.java
│   │   │           │       ├── Auction.java
│   │   │           │       ├── AuctionStatus.java
│   │   │           │       ├── AutoBidConfig.java
│   │   │           │       ├── Bidder.java
│   │   │           │       ├── BidTransaction.java
│   │   │           │       ├── Electronics.java
│   │   │           │       ├── Entity.java
│   │   │           │       ├── Item.java
│   │   │           │       ├── ItemFactory.java
│   │   │           │       ├── Seller.java
│   │   │           │       ├── User.java
│   │   │           │       └── Vehicle.java
│   │   │           │
│   │   │           ├── protocol/
│   │   │           │   ├── MessageType.java
│   │   │           │   ├── Request.java
│   │   │           │   └── Response.java
│   │   │           │
│   │   │           └── server/
│   │   │               ├── controller/
│   │   │               │   ├── AuctionController.java
│   │   │               │   ├── ItemController.java
│   │   │               │   └── UserController.java
│   │   │               ├── dao/
│   │   │               │   ├── AuctionDao.java
│   │   │               │   ├── ItemDao.java
│   │   │               │   └── UserDao.java
│   │   │               ├── observer/
│   │   │               │   ├── AuctionEventManager.java
│   │   │               │   └── AuctionObserver.java
│   │   │               └── service/
│   │   │                   ├── AntiSnipingService.java
│   │   │                   ├── AuctionManager.java
│   │   │                   ├── AutoBidService.java
│   │   │                   ├── AuctionServer.java
│   │   │                   └── ClientHandler.java
│   │   │
│   │   └── resources/                                  <-- THƯ MỤC MỚI ĐƯỢC TẠO
│   │       └── com/
│   │           └── auction/
│   │               └── client/
│   │                   └── view/                       <-- DỜI TOÀN BỘ FILE FXML SANG ĐÂY
│   │                       ├── admin_dashboard.fxml
│   │                       ├── auction_detail.fxml
│   │                       ├── auction_list.fxml
│   │                       ├── login.fxml
│   │                       ├── main.fxml
│   │                       ├── register.fxml
│   │                       └── seller_dashboard.fxml
│   │
│   └── test/
│       └── java/
│           └── com/
│               └── auction/
│                   ├── AuctionManagerTest.java
│                   └── AutoBidServiceTest.java
│
├── pom.xml                                             <-- (Nếu bạn dùng Maven)
└── README.md