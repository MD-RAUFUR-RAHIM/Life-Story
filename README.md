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
