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
