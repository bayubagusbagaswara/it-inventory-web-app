```bash
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
```

# Contoh Lebih Spesifik

```bash
my-springboot-app/
├── .github/
│   └── workflows/
│       └── deploy.yml
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/bayu/myapp/
│   │   │       ├── MyAppApplication.java
│   │   │       ├── controller/
│   │   │       ├── service/
│   │   │       ├── repository/
│   │   │       └── model/
│   │   └── resources/
│   │       └── application.properties
│   └── test/
│       └── java/
│           └── com/bayu/myapp/
│               └── ...
├── pom.xml
└── README.md
```
# Configuration deploy.yml

```bash
name: Build and Deploy Spring Boot to AWS Elastic Beanstalk

on:
  push:
    branches:
      - main     # Jalankan saat ada push ke branch main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout source code
      uses: actions/checkout@v4

    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        distribution: 'temurin'
        java-version: '17'

    - name: Build with Maven
      run: ./mvnw clean package -DskipTests

    - name: Prepare deployment package
      run: |
        mkdir deploy
        cp target/*.jar deploy/application.jar

    - name: Deploy to Elastic Beanstalk
      uses: einaregilsson/beanstalk-deploy@v22
      with:
        aws_access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}         # Secret AWS kamu
        aws_secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}     # Secret AWS kamu
        application_name: 'NAMA_APLIKASI_EB_KAMU'                # Ganti dengan nama aplikasi di Elastic Beanstalk
        environment_name: 'NAMA_ENV_EB_KAMU'                     # Ganti dengan nama environment di Elastic Beanstalk
        region: 'ap-southeast-1'                                 # Ganti sesuai region AWS (contoh: ap-southeast-1 untuk Singapura)
        version_label: ${{ github.sha }}
        deployment_package: deploy/application.jar
```

Bagian yang harus kamu UBAH:

## application_name
Ganti dengan nama aplikasi di Elastic Beanstalk
Contoh:

```yaml
application_name: 'my-springboot-app'
```
## environment_name
Ganti dengan nama environment di Elastic Beanstalk
Contoh:

```yaml
environment_name: 'my-springboot-app-env'
```
## region
Ganti sesuai region AWS tempat aplikasi kamu berada
Contoh region:
```bash
ap-southeast-1 (Singapore)
us-east-1 (N. Virginia)
ap-northeast-1 (Tokyo)
```

## AWS Secrets
Pastikan kamu sudah menyimpan AWS_ACCESS_KEY_ID dan AWS_SECRET_ACCESS_KEY di GitHub repo kamu
`(Settings > Secrets > Actions)`

Penjelasan Step Lainnya:
- Checkout source code: Mengambil kode dari repo.
- Set up JDK 17: Menyiapkan Java 17.
- Build with Maven: Build project jadi file JAR.
- Prepare deployment package: Menyalin file hasil build ke folder deploy.
- Deploy to Elastic Beanstalk: Upload dan deploy file JAR ke AWS Beanstalk.
- Jika belum tahu nama application/environment, cek di AWS Elastic Beanstalk Console.
