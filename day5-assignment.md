# DAY 5 ASSIGNMENT

## A) User Manual Procedure: Create and Activate a Python Virtual Environment

### Purpose

This procedure explains how a first-semester computing student can create a Python virtual environment, activate it, and install a package in the environment.

### Prerequisites

Before starting, the reader needs:

- A computer running Windows, macOS, or Linux.
- Python 3 installed and available from the command line.
- A terminal application, such as Windows Terminal, Terminal, or a command prompt.
- An internet connection for the package installation step.
- Permission to create a folder in the current project directory.

### Procedure

1. **Open a terminal.** Open the terminal application. **Expected result:** A terminal window is open.
2. **Change to the project folder.** Run `cd <project-folder-path>`. **Expected result:** The terminal displays the project folder as its working directory.
3. **Create the virtual environment.** Run `python -m venv .venv`. **Expected result:** A `.venv` directory is created.
4. **Activate the virtual environment.** Run the activation command for the operating system: `.venv\Scripts\Activate.ps1` on Windows PowerShell, `source .venv/bin/activate` on macOS or Linux, or `.venv\Scripts\activate` on Windows Command Prompt. **Expected result:** The terminal prompt includes the virtual environment name.
5. **Check the Python version.** Run `python --version`. **Expected result:** The displayed Python version is the version used to create the environment.
6. **Install the package.** Run `python -m pip install requests`. **Expected result:** The terminal reports that the package was installed successfully.
7. **Check the package installation.** Run `python -m pip show requests`. **Expected result:** The terminal displays the package name and version.
8. **Test the package import.** Run `python -c "import requests; print(requests.__version__)"`. **Expected result:** The terminal displays the installed requests version without an error.

### Screenshot Description

Include a screenshot of the terminal after activation. The screenshot should show the prompt beginning with `.venv` or `(.venv)`, the Python command, and the resulting version or package information. The screenshot should not include passwords, account details, or personal information.

### Troubleshooting

**Common error: The activation command is not recognized.**

If the terminal reports that the activation script cannot be found or that the command is not recognized, confirm that the terminal is in the project folder and that the virtual environment was created successfully. On Windows PowerShell, use `Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass`, then run the activation command again. On macOS or Linux, use `source .venv/bin/activate` rather than running the virtual environment folder directly.

---

## B) API Reference Entry: Create a Task

### Endpoint

- **HTTP method:** `POST`
- **Endpoint path:** `/projects/{project_id}/tasks`
- **Description:** Creates a new task in a specified project for an authenticated user. The task is stored with its title, optional description, assignee, due date, and priority level.

### Request Parameters

| Parameter | Location | Type | Required | Description |
|---|---|---|---|---|
| `project_id` | Path | String | Yes | The unique identifier of the project in which the new task will be created. |
| `title` | Body | String | Yes | The task title. It must contain between 1 and 100 characters. |
| `description` | Body | String | No | A detailed description of the task. It may contain up to 2,000 characters. |
| `assignee_id` | Body | String | No | The user ID of the person assigned to complete the task. If omitted, the task is assigned to no one. |
| `due_date` | Body | String | No | The date by which the task must be completed. The value must use the ISO 8601 date format, such as `2026-11-15`. |
| `priority` | Body | String | No | The task priority. The allowed values are `low`, `medium`, or `high`. The default value is `medium`. |

### Request Headers

| Header | Required | Description |
|---|---|---|
| `Authorization` | Yes | A bearer access token that identifies the authenticated user. The value must use the format `Bearer <access_token>`. |
| `Content-Type` | Yes | The media type of the request body. Set this header to `application/json`. |
| `Accept` | No | The response media type requested by the client. Set this header to `application/json` to receive the documented response format. |

### Example Request

```http
POST /projects/proj_01J8Z4K2M7/tasks HTTP/1.1
Host: api.example.com
Authorization: Bearer eyJhbGciOi...access_token
Content-Type: application/json
Accept: application/json
```

```json
{
  "title": "Prepare project presentation",
  "description": "Summarize the project results and prepare the final presentation slides.",
  "assignee_id": "usr_01J8Z4K2M7",
  "due_date": "2026-11-15",
  "priority": "high"
}
```

### Response Codes

| Status code | Description |
|---|---|
| `201 Created` | The task was created successfully. The response includes the new task's data. |
| `400 Bad Request` | The request body is invalid, a required field is missing, or a field contains an unsupported value. |
| `401 Unauthorized` | The bearer access token is missing, expired, or invalid. |
| `403 Forbidden` | The authenticated user does not have permission to create a task in the specified project. |
| `404 Not Found` | The specified project does not exist or is not available to the authenticated user. |
| `409 Conflict` | A task with the same title and due date already exists in the project. |
| `422 Unprocessable Entity` | The request structure is valid JSON, but one or more field values fail validation. |
| `500 Internal Server Error` | The server encountered an unexpected error while creating the task. |

### Example Successful Response

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /projects/proj_01J8Z4K2M7/tasks/task_01J8Z4K2M8
```

```json
{
  "id": "task_01J8Z4K2M8",
  "project_id": "proj_01J8Z4K2M7",
  "title": "Prepare project presentation",
  "description": "Summarize the project results and prepare the final presentation slides.",
  "assignee_id": "usr_01J8Z4K2M7",
  "due_date": "2026-11-15",
  "priority": "high",
  "status": "pending",
  "created_at": "2026-10-08T14:30:00Z",
  "updated_at": "2026-10-08T14:30:00Z"
}
```

### Response Fields

| Field | Type | Description |
|---|---|---|
| `id` | String | The unique identifier of the created task. |
| `project_id` | String | The identifier of the project that contains the task. |
| `title` | String | The task title. |
| `description` | String | The task description, or an empty string when no description was supplied. |
| `assignee_id` | String or null | The assigned user ID, or `null` when no assignee was provided. |
| `due_date` | String or null | The due date in ISO 8601 format, or `null` when no due date was supplied. |
| `priority` | String | The task priority: `low`, `medium`, or `high`. |
| `status` | String | The current task status. This example uses `pending`. |
| `created_at` | String | The UTC timestamp when the task was created. |
| `updated_at` | String | The UTC timestamp when the task was last updated. |
