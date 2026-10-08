# Lab WinForms Workflow

## Purpose

Use **openSUSE as the main coding environment** and **Windows 10 KVM as the WinForms UI/design environment**.

The workflow is designed to:
- keep UI work separate from application logic,
- reduce Git conflicts between Linux and Windows,
- make each assignment easier to test,
- keep the whole lab inside one WinForms project.

---

## Environment Split

### openSUSE
Use for:
- C# logic
- validation
- helper functions
- tests
- Git operations
- code review/refactoring

Avoid editing:
- `*.Designer.cs`
- `*.resx`

### Windows 10 KVM
Use for:
- Visual Studio
- WinForms Designer
- creating Forms
- placing controls
- wiring UI events
- running/debugging the final Windows application

---

## Recommended Project Structure

```text
Lab01/
├── Lab01.csproj
├── Program.cs
│
├── Forms/
│   ├── MainForm.cs
│   ├── MainForm.Designer.cs
│   ├── MainForm.resx
│   ├── Lab01_Bai01.cs
│   ├── Lab01_Bai01.Designer.cs
│   ├── Lab01_Bai01.resx
│   ├── ...
│
├── Logic/
│   ├── Bai01Logic.cs
│   ├── Bai02Logic.cs
│   ├── Bai03Logic.cs
│   ├── Bai04Logic.cs
│   └── CalculatorLogic.cs
│
└── README.md
```

---

## General Workflow for Every Assignment

### 1. Read Requirement
Identify:
- input,
- expected output,
- required C# concept,
- validation rules,
- optional/extension requirements.

### 2. Define Logic
Before touching the UI, define:
- methods needed,
- input types,
- return values,
- edge cases.

### 3. Implement Logic on openSUSE
Create or update:

```text
Logic/BaiXXLogic.cs
```

Keep methods independent from WinForms controls whenever possible.

Example:

```csharp
public static double FindMax(double a, double b, double c)
{
    // logic only
}
```

### 4. Test Logic
Check at least:
- normal input,
- zero,
- negative values,
- decimal values if allowed,
- invalid input,
- empty input,
- boundary values.

### 5. Commit Logic

```bash
git add .
git commit -m "feat(baiXX): implement core logic"
git push
```

### 6. Pull on Windows

```powershell
git pull
```

### 7. Build UI in Visual Studio
Create the Form and add required controls.

Typical controls:
- `Label`
- `TextBox`
- `Button`
- `ComboBox`
- `GroupBox`

### 8. Connect UI to Logic
Keep event handlers thin.

Good:

```csharp
private void btnCalculate_Click(object sender, EventArgs e)
{
    Calculate();
}
```

Avoid putting all business logic directly inside button handlers.

### 9. Add Input Validation
Prefer:

```csharp
int.TryParse(...)
double.TryParse(...)
decimal.TryParse(...)
```

instead of unsafe direct parsing.

### 10. Run on Windows
Verify:
- application launches,
- form opens,
- controls work,
- invalid input is handled,
- clear/reset works,
- exit works,
- repeated usage does not break state.

### 11. Commit UI

```powershell
git add .
git commit -m "ui(baiXX): complete WinForms interface"
git push
```

### 12. Final Regression Check
After pulling changes back to Linux or switching assignments:
- make sure previous forms still compile,
- make sure navigation still works,
- verify no accidental changes to other assignments.

---

## Git Rules

### Rule 1
Do not use one shared working directory between Linux and Windows.

Use two clones:

```text
openSUSE:
~/projects/Lab01

Windows:
C:\Projects\Lab01
```

### Rule 2
Linux should avoid editing:

```text
*.Designer.cs
*.resx
```

### Rule 3
Commit small changes.

Examples:

```text
feat(bai01): implement min max logic
ui(bai01): create assignment form
fix(bai01): validate invalid numeric input
test(bai01): add edge case checks
```

### Rule 4
Pull before starting work on either OS.

---

## Assignment Completion Checklist

For every assignment:

- [ ] Requirement understood
- [ ] Input/output defined
- [ ] Logic implemented
- [ ] Logic tested
- [ ] UI created
- [ ] UI connected to logic
- [ ] Input validation added
- [ ] Clear/reset works
- [ ] Exit/navigation works
- [ ] Tested on Windows
- [ ] Git commit created
- [ ] Git pushed
- [ ] Previous assignments still work

---

## Recommended Work Order

```text
Project setup
    ↓
Main navigation form
    ↓
Assignment 1
    ↓
Assignment 2
    ↓
Assignment 3
    ↓
Assignment 4
    ↓
Assignment 5
    ↓
Extensions
    ↓
Full regression test
    ↓
Submission package
```

Do the required functionality first. Only work on extensions after the base requirements of all assignments are complete.
