using lab13v1.Data;
using lab13v1.Models;
using lab13v1.Requests;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;

namespace lab13v1.Controllers
{
    [Route("api/[controller]/[action]")]
    [ApiController]
    public class ProductsCustomController : ControllerBase
    {

        private readonly CustomerContext _context;
        public ProductsCustomController(CustomerContext context)
        {
            _context = context;
        }
        [HttpPost]
        public void Insert(ProductRequestV1 request)
        {


           _context.Products.Add(new Product
           {
               Name = request.Name,
               Price = request.Price,
               IsActive = true // Por defecto, al crear un producto, está activo
           });
         _context.SaveChanges();

        }

        [HttpPost]
        public void InsertGroup(List<ProductRequestV1> request)
        {


            var model= request.Select(x => new Product
            {
                Name = x.Name,
                Price = x.Price,
                IsActive = true // Por defecto, al crear un producto, está activo
            }).ToList();

            _context.Products.AddRange(model);
            _context.SaveChanges();


        }
    }
}
