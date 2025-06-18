nama-project/
├── .github/
│   └── workflows/
│       └── deploy.yml         <-- File workflow GitHub Actions untuk CI/CD
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/
│   │   │       └── example/
│   │   │           └── namaproject/
│   │   │               ├── NamaProjectApplication.java  <-- Main class (entry point)
│   │   │               ├── controller/                 <-- REST Controllers
│   │   │               ├── service/                    <-- Service Layer
│   │   │               ├── repository/                 <-- Repository/Data Access
│   │   │               └── model/ (atau entity/)       <-- Model/Entity class
│   │   └── resources/
│   │       ├── application.properties (atau .yml)      <-- Konfigurasi aplikasi
│   │       ├── static/                                <-- Static file (css/js/img)
│   │       └── templates/                             <-- Template (Thymeleaf, dll)
│   └── test/
│       └── java/
│           └── com/
│               └── example/
│                   └── namaproject/
│                       └── ...                        <-- Test class
├── pom.xml      <-- Konfigurasi Maven (atau build.gradle jika pakai Gradle)
└── README.md    <-- Dokumentasi
