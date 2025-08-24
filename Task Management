import streamlit as st
import gspread
from google.oauth2.service_account import Credentials
import pandas as pd

# ---- Connect to Google Sheet ----
scope = ["https://spreadsheets.google.com/feeds","https://www.googleapis.com/auth/drive"]
creds = Credentials.from_service_account_file("service_account.json", scopes=scope)
client = gspread.authorize(creds)

# Open Google Sheet
SHEET_NAME = "Global Business Konsultancy"
sheet = client.open(SHEET_NAME)

# Load data
tasks_ws = sheet.worksheet("Tasks")
tasks_data = tasks_ws.get_all_records()
df_tasks = pd.DataFrame(tasks_data)

# ---- Streamlit App ----
st.title("📌 Task Management Dashboard")

# Show pending/completed
status_filter = st.selectbox("Filter by Status", ["All", "Pending", "Completed", "Overdue"])
if status_filter != "All":
    df_tasks = df_tasks[df_tasks["Status"] == status_filter]

st.dataframe(df_tasks)

# ---- Update Task ----
st.subheader("✅ Update Task Status")
task_ids = df_tasks["Task_ID"].tolist()
selected_task = st.selectbox("Select Task", task_ids)

new_status = st.radio("New Status", ["Pending", "Completed"])
if st.button("Update Status"):
    cell = tasks_ws.find(selected_task)
    row = cell.row
    tasks_ws.update_cell(row, df_tasks.columns.get_loc("Status") + 1, new_status)
    st.success("Task updated successfully!")
