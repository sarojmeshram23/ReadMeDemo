1. Create ASP.NET Core Empty project.
2. Keep only solution and delete everything.
3. Right click open explorer and remove the folder specific to this project, keep the solution file as it is.
4. Create Folder as follows,
    a. FrontEnd
    b. Gateway
    c. Integration
    d. Services -> Here we will add API endpoints.
5. right click on services -> create ASP.net Core Web API  -> Mango.Services.CouponAPI
(We can update the port number as shown in the diagram i.e. 7001 Properties/launchSettings.json 
inside "https" section "ApplicationUrl"->add portnumber here.)

6. Add "Models" folder -> Add class as "Coupon.cs" {int CouponId, string CouponCode, double DiscountAmount, int minAmount}
   Add Dto folder inside Models(Models/Dto) -> Create CouponDto.cs class(properties similar to Coupon.cs file) 

7. Package Automapper is responsible for map one object to other.
   Add NugetPackages 
   Right click on "Mango.Services.CouponAPI" -> Manage Nuget Packages -> Browse -> write "Automapper" -> install it.
   Automapper.Extensions.Microsoft.DependecyInjection 
   Microsoft.EntityFrameoworkCore.SqlServer 
   Microsoft.EntityFrameoworkCore
   Microsoft.EntityFrameoworkCore.Tools
   Microsoft.AspNetCore.Authentication.JwtBearer
8. Add Data folder , add class as "AppDbContext.cs"

public class AppDbContext: DbContext
{
        //Default setting for .net core
	public AppDbContext(DbContextOptions<AppDbContext> options):base(options)
	{}
        public DbSet<Coupon> Coupons {get; set;}
}

9. Add keys to above coupon class as follows,

{[Key]int CouponId, [Required]string CouponCode, [Required]double DiscountAmount,[Required]int minAmount}

10. Add ConnectionStrings in appSettings.json
"ConnectionStrings" :{"DefaultConnection":"server:(LocalDb)\\MSSQLLocalDB;Database=MangoCoupon;Trusted_Connection:True;TrustServerConnection:True"}

11. Inside Program.cs,

builder.Services.AddDbContext<AddDbContext>(option => 
{
	option.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")); //retrieve the connection 
});

12. Go to package manager console // This is to create Coupon table in database.
> add-migration AddCouponToDb   //This will add Coupon table in database
> update-databse

13. //Seed Database
Inside AppDbContext.cs file, add below method

Start Video 18