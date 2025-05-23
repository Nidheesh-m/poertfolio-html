# Electricity Bill Calculator Web Application

This project is a simple, yet robust, web application designed to calculate electricity energy charges based on consumed units. It demonstrates a clear separation of concerns, API-driven data fetching, and a fully database-driven tariff and billing structure.

## Project Overview & Features

The application provides a user-friendly form to input electricity consumption units and instantly calculates the total energy charges, including fixed charges, based on predefined tariff structures.

Key features and fulfilled requirements include:

* **Web Application Form:** A clean and responsive web form to accept user input for consumed units.

* **Technology Stack:** Developed using **.NET 8** (ASP.NET Core MVC) for the backend and standard **HTML, CSS, and JavaScript (Fetch API)** for the frontend.

* **Database Integration:** Utilizes **Entity Framework Core with MSSQL Server** for robust data persistence.

* **Dynamic Tariff & Pricing:** All tariff ranges, per-unit rates (telescopic and non-telescopic), and fixed charges are stored and fetched dynamically from the database. **No tariff rates or ranges are hardcoded in the application's business logic.**

* **Backend APIs:** RESTful API endpoints are exposed to:

  * Fetch available tariffs (e.g., "LT-1A").

  * Fetch available purposes (e.g., "Domestic").

  * Calculate the electricity bill based on consumed units and applicable rates.

* **Frontend Consumption:** The frontend interacts with these backend APIs using JavaScript's `fetch` API to dynamically populate dropdowns and display calculation results.

* **Bill Calculation Logic:** Implements the KSEB electricity bill calculation rules, distinguishing between:

  * **Telescopic Rates (up to 250 units):** Different per-unit rates apply in blocks (0-50, 51-100, 101-150, 151-200, 201-250).

  * **Non-Telescopic Rates (over 250 units):** A single flat rate applies to all consumed units based on the total consumption slab (0-300, 301-350, 351-400, 401-500, >500).

  * A fixed charge of ₹35 is added to all bills.

* **Database Migrations & Seeding:** Database schema is managed using Entity Framework Core Migrations, and initial tariff, purpose, slab rate, and fixed charge data is seeded upon database creation.

* **Database Indexing:** Proper indexing is applied to the `SlabRates` table to optimize tariff lookup performance.

## Application Architecture

The project follows the Model-View-Controller (MVC) architectural pattern:

* **Models:** C# classes representing the database entities (`Tariff`, `Purpose`, `SlabRate`, `FixedCharge`).

* **Views:** Razor (`.cshtml`) files defining the user interface (e.g., `Index.cshtml`).

* **Controllers:** Handle HTTP requests, interact with the database via Entity Framework Core, apply business logic, and return data (for API endpoints) or views.

## Database Schema Overview

The application utilizes the following tables in MSSQL Server:

* `Tariffs`: Stores different electricity tariff types (e.g., LT-1A).

* `Purposes`: Stores different consumption purposes (e.g., Domestic).

* `SlabRates`: Stores the detailed per-unit rates for both telescopic and non-telescopic billing, linked to specific tariffs.

* `FixedCharges`: Stores fixed charge amounts.

* `__EFMigrationsHistory`: Entity Framework Core's internal table to track applied migrations.

## Setup and Running the Application

To set up and run this project locally, please ensure you have the following prerequisites installed:

1. **Prerequisites:**

   * **.NET 8 SDK** (or higher): Download from [Microsoft .NET Website](https://dotnet.microsoft.com/download/dotnet/8.0).

   * **Visual Studio 2022:** (Community Edition is free) - Recommended IDE for .NET development.

   * **SQL Server:** A local instance of MSSQL Server (e.g., SQL Server Express, SQL Server Developer Edition).

   * **SQL Server Management Studio (SSMS):** (Optional, but recommended for database inspection).

2. **Clone or Unzip the Project:**

   * If you received a Git repository, clone it to your local machine.

   * If you received a ZIP file, extract its contents to your desired development directory.

3. **Configure Database Connection:**

   * Open the `appsettings.json` file located in the project's root directory.

   * Locate the `"ConnectionStrings"` section.

   * Update the `"DefaultConnection"` string to point to your local MSSQL Server instance.

     * **Example for LocalDB (common with Visual Studio):**
       `"Server=(localdb)\\MSSQLLocalDB;Database=ElectricityBillDb;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=True"`

     * **Example for Named SQL Server Instance:**
       `"Server=YourComputerName\\SQLEXPRESS;Database=ElectricityBillDb;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=True"`

     * **Example for SQL Server Authentication:**
       `"Server=YourComputerName;Database=ElectricityBillDb;User ID=your_username;Password=your_password;MultipleActiveResultSets=true;TrustServerCertificate=True"`

4. **Open in Visual Studio:**

   * Navigate to the root directory of the extracted project (where `ElectricityBillCalculator.sln` is located).

   * Double-click `ElectricityBillCalculator.sln` to open the solution in Visual Studio.

5. **Restore NuGet Packages:**

   * Visual Studio should automatically restore NuGet packages upon opening the solution. If not, right-click on the `ElectricityBillCalculator` solution in Solution Explorer and select "Restore NuGet Packages."

6. **Apply Database Migrations:**

   * Open the **Package Manager Console** in Visual Studio (`Tools` > `NuGet Package Manager` > `Package Manager Console`).

   * Ensure `ElectricityBillCalculator` is selected as the "Default project" in the console dropdown.

   * Run the following command to apply the initial database migration (which creates tables and seeds data):

     ```
     Update-Database
     
     ```

   * This will create the `ElectricityBillDb` database (if it doesn't exist) and populate it with the necessary tariff, purpose, slab rate, and fixed charge data.

7. **Run the Application:**

   * Once all steps are complete, press `F5` in Visual Studio or click the green "IIS Express" (or "http") button to run the application.

   * The application should open in your default web browser at `https://localhost:XXXX/` (where `XXXX` is a dynamically assigned port number).

## Key Project Components

* **`Controllers/BillCalculatorController.cs`**: Contains the API endpoints for fetching data and performing the bill calculation logic.

* **`Models/`**: Defines the C# classes (`Tariff`, `Purpose`, `SlabRate`, `FixedCharge`) that map to database tables.

* **`Data/ApplicationDbContext.cs`**: The Entity Framework Core DbContext, configuring database mapping, indexing, and initial data seeding.

* **`Views/Home/Index.cshtml`**: The main Razor View that renders the HTML structure of the calculator form and bill details.

* **`wwwroot/css/billCalculator.css`**: Custom CSS rules providing the responsive layout and styling for the application's user interface.

* **`wwwroot/js/billCalculator.js`**: Client-side JavaScript responsible for fetching data from the APIs, handling user input, initiating calculations, and dynamically updating the UI.