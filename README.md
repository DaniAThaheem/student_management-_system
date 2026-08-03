# 🎓 DaniSfinalProject — Student Management System

![.NET Framework](https://img.shields.io/badge/.NET%20Framework-4.7.2-512BD4?logo=dotnet&logoColor=white)
![Platform](https://img.shields.io/badge/platform-Windows-0078D6?logo=windows&logoColor=white)
![Language](https://img.shields.io/badge/language-C%23-239120?logo=csharp&logoColor=white)
![Database](https://img.shields.io/badge/database-SQL%20Server-CC2927?logo=microsoftsqlserver&logoColor=white)
![Build](https://img.shields.io/badge/build-passing-brightgreen)
![License](https://img.shields.io/badge/license-MIT-yellow)

A **Windows Forms (WinForms) desktop application** for managing student academic records — built with C#, ADO.NET, and Microsoft SQL Server. The system provides a secure, MDI-based administrative interface for handling student enrollment, semester tracking, and record lookup in an academic institution setting.

This project was developed as a final-year academic application and serves as a practical reference for **ADO.NET data access patterns, multi-form MDI application architecture, and typed DataSet usage** in classic WinForms development.

---

## 📑 Table of Contents

- [Features](#-features)
- [System Requirements](#-system-requirements)
- [Installation & Setup](#-installation--setup)
- [Database Setup](#-database-setup)
- [Usage](#-usage)
- [Project Structure](#-project-structure)
- [Configuration](#-configuration)
- [Troubleshooting](#-troubleshooting)
- [Known Limitations & Roadmap](#-known-limitations--roadmap)
- [Contributing](#-contributing)
- [License](#-license)

---

## ✨ Features

- **🔐 Admin Authentication** — Secure login gate (`frmLogin`) with lockout after 3 failed attempts and a dedicated admin registration form (`frmRegisterAdmin`).
- **🗂️ MDI Dashboard** — A single main window (`frmMain`) hosts all child forms via a menu-driven MDI interface (Insert / View / Edit / Remove / Search).
- **➕ Student Record Management** — Add, update, and delete student records (`frmAddStudent`, `frmUpdateStudent`, `DeleteStudent`) with duplicate roll-number checks.
- **📋 Record Viewing** — Tabular, grid-based views of all student records (`ViewStudent`).
- **🔍 Multi-Criteria Search** — Look up students by Roll Number, Name, or Address (`frmSearchByRollNo`, `frmSearchByName`, `frmSearchByAddress`).
- **📚 Semester & Enrollment Tracking** — Manage semester (`Smester`/`frmSmester`) and course enrollment (`Enrollment`/`frmEnrollment`) data tied to each student.
- **🔒 Session Handling** — Dedicated logout flow for administrators (`frmLogoutAdmin`).
- **🗄️ Typed DataSets** — Strongly-typed ADO.NET DataSets (`StudentsDataSet`, `StudentsDataSet1–3`) generated via the Visual Studio Data Source wizard for compile-time-safe data binding.

---

## 🖥️ System Requirements

| Requirement | Details |
|---|---|
| **OS** | Windows 10 or Windows 11 (Windows 7/8.1 also supported with .NET Framework 4.7.2 installed) |
| **.NET Framework** | 4.7.2 (targeted in `DaniSfinalProject.csproj`) |
| **IDE** | Visual Studio 2019 or later (Community Edition is sufficient) — must include the **.NET desktop development** workload |
| **Database** | Microsoft SQL Server (LocalDB, Express, or full edition) |
| **Data Provider** | `System.Data.SqlClient` (included with .NET Framework — no NuGet install required) |
| **Workload** | "Windows Forms App (.NET Framework)" component in Visual Studio Installer |

> ⚠️ This is a **.NET Framework** (not .NET Core / .NET 5+) WinForms project. It will **not** open directly in `dotnet` CLI-based tooling — Visual Studio (or MSBuild with the Framework toolset) is required to build it.

---

## 🚀 Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/DaniSfinalProject.git
cd DaniSfinalProject
```

### 2. Open the solution

Open `DaniSfinalProject.sln` in Visual Studio. Visual Studio will automatically detect the project as a .NET Framework 4.7.2 WinForms application.

### 3. Restore dependencies

This project uses only built-in .NET Framework assemblies (`System.Data.SqlClient`, `System.Windows.Forms`, etc.) — there are **no external NuGet packages** to restore. If your fork later adds NuGet dependencies, restore them with:

```bash
nuget restore DaniSfinalProject.sln
```

or via Visual Studio: **right-click the Solution → Restore NuGet Packages**.

### 4. Build the project

**Visual Studio:** `Build → Build Solution` (or `Ctrl+Shift+B`)

**Command line (Developer Command Prompt / MSBuild):**

```bash
msbuild DaniSfinalProject.sln /p:Configuration=Debug /p:Platform="Any CPU"
```

### 5. Run the application

Press **F5** in Visual Studio, or launch the compiled binary directly:

```bash
DaniSfinalProject\bin\Debug\DaniSfinalProject.exe
```

---

## 🗄️ Database Setup

The application connects to a SQL Server database named **`Students`** via a connection string defined in `App.config`:

```xml
<connectionStrings>
    <add name="DaniSfinalProject.Properties.Settings.StudentsConnectionString"
         connectionString="Data Source=DESKTOP-8BL3MIG;Initial Catalog=Students;Integrated Security=True;TrustServerCertificate=True"
         providerName="System.Data.SqlClient" />
</connectionStrings>
```

Before running the app, you'll need to:

1. **Create the `Students` database** on your local SQL Server instance (or LocalDB).
2. **Create the required tables** — at minimum: `Admin`, `Student`, `Smester`, and `Enrollment` (inferred from the data-access classes `Admin.cs`, `Student.cs`, `Smester.cs`, `Enrollment.cs`).
3. **Seed at least one admin account** so you can log in — insert directly into the `Admin` table:

   ```sql
   INSERT INTO Admin (userID, password) VALUES ('admin', 'admin123');
   ```

4. **Update the connection string** in both `App.config` and `DBConnection.cs` to point at your own SQL Server instance (see [Configuration](#-configuration) below).

> 💡 **Tip:** If you don't have a full SQL Server install, [SQL Server Express LocalDB](https://learn.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb) is a lightweight, free alternative that works well for local development.

---

## 🖱️ Usage

1. **Launch the app** → the login screen (`frmLogin`) appears.
2. **Sign in** with valid admin credentials. Three failed attempts will close the application automatically.
3. On success, the **MDI main window** (`frmMain`) opens with a top menu bar:
   - **Insert** → Add Student Record / Add Semester Record / Add Grades
   - **View** → Student Record / Course Record / Grades Record
   - **Edit** → Update Student Info / Update Grades / Update Course Info
   - **Remove** → Student / Grades / Course
   - **Search** → by Roll No / Name / Address
4. Each menu item opens the corresponding child form **inside the MDI container**, allowing you to work across multiple records without leaving the main window.

<!-- 📸 Screenshot placeholder: Login screen -->
`![Login Screen](docs/screenshots/login.png)`

<!-- 📸 Screenshot placeholder: Main MDI dashboard with menu expanded -->
`![Main Dashboard](docs/screenshots/dashboard.png)`

<!-- 📸 Screenshot placeholder: Add Student form -->
`![Add Student](docs/screenshots/add-student.png)`

<!-- 🎞️ GIF placeholder: End-to-end workflow (login → add → search → update) -->
`![Full Workflow Demo](docs/screenshots/workflow-demo.gif)`

> Replace the placeholders above with real captures once available, and store them under a `docs/screenshots/` folder in the repo root.

---

## 🗂️ Project Structure

```
DaniSfinalProject/
├── DaniSfinalProject.sln              # Visual Studio solution file
└── DaniSfinalProject/
    ├── Program.cs                     # Application entry point (Main) — launches frmLogin
    ├── App.config                     # Connection strings & runtime configuration
    ├── DBConnection.cs                # Static SqlConnection wrapper (Connect/GetConnection/Disconnect)
    │
    ├── Domain / Data-Access Classes
    │   ├── Admin.cs                   # Admin authentication & registration queries
    │   ├── Student.cs                 # CRUD + search queries for the Student table
    │   ├── Smester.cs                 # Semester record queries
    │   └── Enrollment.cs              # Enrollment record queries
    │
    ├── Forms (UI Layer — each with a paired .Designer.cs and .resx)
    │   ├── frmLogin.cs / .Designer.cs / .resx        # Admin login screen
    │   ├── frmRegisterAdmin.cs / ...                 # New admin account registration
    │   ├── Form1.cs (frmMain) / .Designer.cs / .resx # MDI parent / main dashboard
    │   ├── frmAddStudent.cs / ...                    # Add new student record
    │   ├── frmUpdateStudent.cs / ...                 # Edit existing student record
    │   ├── DeleteStudent.cs / ...                    # Remove student record
    │   ├── ViewStudent.cs / ...                      # Grid view of all students
    │   ├── frmSearchByRollNo.cs / ...                # Search by roll number
    │   ├── frmSearchByName.cs / ...                  # Search by name
    │   ├── frmSearchByAddress.cs / ...                # Search by address
    │   ├── frmSmester.cs / ...                        # Semester record management
    │   ├── frmEnrollment.cs / ...                     # Enrollment record management
    │   └── frmLogoutAdmin.cs / ...                    # Admin logout confirmation
    │
    ├── Typed DataSets
    │   ├── StudentsDataSet.xsd / .Designer.cs / .xsc / .xss
    │   ├── StudentsDataSet1.xsd / .Designer.cs / .xsc / .xss
    │   ├── StudentsDataSet2.xsd / .Designer.cs / .xsc / .xss
    │   └── StudentsDataSet3.xsd / .Designer.cs / .xsc / .xss
    │
    └── Properties/
        ├── AssemblyInfo.cs            # Assembly metadata
        ├── Resources.resx / Resources.Designer.cs   # Embedded resources (icons, images)
        └── Settings.settings / Settings.Designer.cs # Strongly-typed app settings
```

### Architectural notes

- **Pattern:** The project follows a loose **two-layer** structure — plain-old C# classes (`Admin`, `Student`, `Smester`, `Enrollment`) handle direct ADO.NET data access, while WinForms classes handle presentation and event wiring. There is no separate repository/service abstraction layer, which makes this a good learning reference for understanding raw ADO.NET before moving to patterns like Repository or Entity Framework.
- **Navigation:** `frmMain` acts as the **MDI container**; all functional forms set `MdiParent = this` and are opened via `.Show()` from menu click handlers.
- **Connection lifecycle:** A single static `SqlConnection` is opened once in `DBConnection.Connect()` (called from `frmLogin_Load`) and reused across all forms via `DBConnection.GetConnection()`.

---

## ⚙️ Configuration

### `App.config` connection string

Located at `DaniSfinalProject/App.config`. Update the `Data Source` and `Initial Catalog` values to match your environment:

```xml
<add name="DaniSfinalProject.Properties.Settings.StudentsConnectionString"
     connectionString="Data Source=YOUR_SERVER_NAME;Initial Catalog=Students;Integrated Security=True;TrustServerCertificate=True"
     providerName="System.Data.SqlClient" />
```

### Hard-coded connection string in `DBConnection.cs`

⚠️ Note that `DBConnection.cs` currently **duplicates** the connection string as a hard-coded literal rather than reading it from `App.config` / `Properties.Settings`:

```csharp
string connectionString = "Data Source=DESKTOP-8BL3MIG;Initial Catalog=Students;Integrated Security=True;TrustServerCertificate=True";
```

If you fork this project, consider refactoring `DBConnection.Connect()` to read from `ConfigurationManager.ConnectionStrings[...]` instead, so the connection only needs to be updated in one place:

```csharp
string connectionString = ConfigurationManager
    .ConnectionStrings["DaniSfinalProject.Properties.Settings.StudentsConnectionString"]
    .ConnectionString;
```

(This requires adding a reference to `System.Configuration`.)

### Authentication mode

The default connection string uses **Integrated Security (Windows Authentication)**. If your SQL Server instance uses SQL Authentication instead, update the string to:

```
Data Source=YOUR_SERVER;Initial Catalog=Students;User ID=your_user;Password=your_password;TrustServerCertificate=True
```

---

## 🛠️ Troubleshooting

| Issue | Likely Cause | Fix |
|---|---|---|
| **"A network-related or instance-specific error..."** | SQL Server instance name in the connection string doesn't match your machine | Update `Data Source` in `App.config` and `DBConnection.cs` to your actual instance name (e.g. `.\SQLEXPRESS` or `(localdb)\MSSQLLocalDB`) |
| **Login screen accepts nothing / always shows "Wrong Credentials"** | `Admin` table is empty or doesn't exist | Create the table and insert a seed admin row (see [Database Setup](#-database-setup)) |
| **Build fails: "The type or namespace 'SqlClient' could not be found"** | Missing reference to `System.Data.SqlClient` | Right-click project → **Add Reference** → check `System.Data` (SqlClient ships with it in .NET Framework) |
| **Designer files won't open / "InitializeComponent" errors** | Visual Studio version mismatch or corrupted `.resx` | Ensure you're using VS2019+ with the WinForms Designer workload; try **Clean → Rebuild** |
| **`TargetFrameworkVersion` not found / project won't load** | .NET Framework 4.7.2 Developer Pack not installed | Install it via [Visual Studio Installer → Individual Components → .NET Framework 4.7.2 targeting pack] |
| **App builds but crashes instantly on launch** | SQL connection fails inside `frmLogin_Load` before any UI renders | Check the exception `MessageBox` text — it will surface the raw ADO.NET error; verify server name, database existence, and firewall/port 1433 access |
| **`obj\`/`bin\` folders cause merge conflicts or bloat the repo** | Build artifacts committed to source control | Add a `.gitignore` excluding `bin/`, `obj/`, and `.vs/` (see below) |
| **`TrustServerCertificate=True` errors on older SQL Server versions** | Option not supported pre-2016 | Remove the flag, or add `Encrypt=False` instead depending on your SQL Server version |

**Recommended `.gitignore` additions:**

```gitignore
bin/
obj/
.vs/
*.user
*.suo
```

---

## 🗺️ Known Limitations & Roadmap

This codebase reflects a functional academic prototype rather than a production-hardened system. Contributors looking for meaningful first issues might tackle:

- [ ] **SQL injection risk** — several data-access methods (e.g. `Admin.GetAdminRecord`, `Student.SearchByName`) build queries via string concatenation. Migrate to parameterized queries (`SqlParameter`) throughout.
- [ ] **Plaintext password storage** — admin passwords are stored and compared as plaintext. Introduce hashing (e.g. BCrypt or PBKDF2).
- [ ] **Centralize the connection string** — remove the duplicated hard-coded string in `DBConnection.cs` in favor of `ConfigurationManager`.
- [ ] **Migrate to .NET (Core) WinForms** for cross-version tooling support and long-term maintainability.
- [ ] **Add automated tests** — currently no unit/integration test project exists.
- [ ] **Grades module** — menu items for "Add/Update/Remove Grades" exist in the UI but aren't yet wired to a backing data class.

---

## 🤝 Contributing

Contributions, bug reports, and feature suggestions are welcome!

1. **Fork** the repository.
2. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. Make your changes, following the existing naming conventions (`frmX` for forms, PascalCase for classes/methods).
4. Test your changes locally against a SQL Server instance.
5. Commit with a clear message:
   ```bash
   git commit -m "Add: parameterized queries for Student search methods"
   ```
6. Push and open a **Pull Request** describing what changed and why.

For larger changes (e.g. migrating to Entity Framework, refactoring the data-access layer, or porting to .NET 8), please open an issue first to discuss the approach.

---

## 📄 License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

```
MIT License — free to use, modify, and distribute with attribution.
```

---

<p align="center">Built with C# & WinForms · Maintained by <a href="https://github.com/DaniAThaheem">@DaniAThaheem</a></p>
