Perfect! Based on your requirement and the models we defined earlier (PersonMaster and PersonDetail), we’ll create:

1. A controller: PersonController.cs


2. Views:

Create.cshtml – for form submission

List.cshtml – to display records





---

1. Controller: PersonController.cs

using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using YourNamespace.Models; // Update with your actual namespace
using YourNamespace.Data;   // Update with your actual DbContext namespace

public class PersonController : Controller
{
    private readonly AppDbContext _context;

    public PersonController(AppDbContext context)
    {
        _context = context;
    }

    // GET: Person/Create
    public IActionResult Create()
    {
        return View(new PersonMaster { PersonDetails = new List<PersonDetail> { new PersonDetail() } });
    }

    // POST: Person/Create
    [HttpPost]
    [ValidateAntiForgeryToken]
    public async Task<IActionResult> Create(PersonMaster person)
    {
        if (ModelState.IsValid)
        {
            _context.tbl_PersonMaster.Add(person);
            await _context.SaveChangesAsync();
            return RedirectToAction(nameof(List));
        }
        return View(person);
    }

    // GET: Person/List
    public async Task<IActionResult> List()
    {
        var data = await _context.tbl_PersonMaster
                                 .Include(p => p.PersonDetails)
                                 .ToListAsync();
        return View(data);
    }
}


---

2. Views

a. Create.cshtml

@model PersonMaster
@{
    ViewData["Title"] = "Create Person";
}

<h2>Create Person</h2>

<form asp-action="Create" method="post">
    <div>
        <label>Name</label>
        <input asp-for="Name" />
        <span asp-validation-for="Name"></span>
    </div>
    <div>
        <label>Phone</label>
        <input asp-for="Phone" />
        <span asp-validation-for="Phone"></span>
    </div>
    <div>
        <label>Email</label>
        <input asp-for="Email" />
        <span asp-validation-for="Email"></span>
    </div>

    <h4>Examinations</h4>
    <table id="examTable">
        <thead>
            <tr>
                <th>Examination</th>
                <th>Passing Year</th>
                <th>GPA</th>
                <th>Action</th>
            </tr>
        </thead>
        <tbody>
            @for (int i = 0; i < Model.PersonDetails.Count; i++)
            {
                <tr>
                    <td><input asp-for="PersonDetails[@i].Examination" /></td>
                    <td><input asp-for="PersonDetails[@i].PassingYear" /></td>
                    <td><input asp-for="PersonDetails[@i].GPA" /></td>
                    <td><button type="button" onclick="removeRow(this)">Remove</button></td>
                </tr>
            }
        </tbody>
    </table>
    <button type="button" onclick="addRow()">Add Row</button>

    <div>
        <button type="submit">Save</button>
        <a asp-action="List">List</a>
    </div>
</form>

@section Scripts {
    <partial name="_ValidationScriptsPartial" />
    <script>
        function addRow() {
            var index = document.querySelectorAll('#examTable tbody tr').length;
            var row = `
                <tr>
                    <td><input name="PersonDetails[${index}].Examination" /></td>
                    <td><input name="PersonDetails[${index}].PassingYear" /></td>
                    <td><input name="PersonDetails[${index}].GPA" /></td>
                    <td><button type="button" onclick="removeRow(this)">Remove</button></td>
                </tr>`;
            document.querySelector("#examTable tbody").insertAdjacentHTML('beforeend', row);
        }

        function removeRow(button) {
            button.closest('tr').remove();
        }
    </script>
}


---

b. List.cshtml

@model List<PersonMaster>
@{
    ViewData["Title"] = "Person List";
}

<h2>Person List</h2>

<table border="1">
    <thead>
        <tr>
            <th>Name</th>
            <th>Phone</th>
            <th>Email</th>
            <th>Exams</th>
        </tr>
    </thead>
    <tbody>
    @foreach (var person in Model)
    {
        <tr>
            <td>@person.Name</td>
            <td>@person.Phone</td>
            <td>@person.Email</td>
            <td>
                <table>
                    @foreach (var exam in person.PersonDetails)
                    {
                        <tr>
                            <td>@exam.Examination</td>
                            <td>@exam.PassingYear</td>
                            <td>@exam.GPA</td>
                        </tr>
                    }
                </table>
            </td>
        </tr>
    }
    </tbody>
</table>


---

Let me know if you want to add edit/delete functionality or if you'd like this to be made in Razor Pages instead of MVC.

