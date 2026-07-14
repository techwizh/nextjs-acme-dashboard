# Acme Dashboard — User Manual

Welcome to **Acme Dashboard**, a web application for managing invoices and customers. This guide explains how to use the app day to day.

## Live app

**URL:** [nextjs-dashboard-ten-sigma-u5e22t1u16.vercel.app](https://nextjs-dashboard-ten-sigma-u5e22t1u16.vercel.app)

## Logging in

1. Open the live URL (or your local app at `http://localhost:3000`).
2. You are redirected to the **Login** page if you are not signed in.
3. Enter your email and password.
4. Click **Log in**.

**Demo credentials:**

| Field    | Value              |
|----------|--------------------|
| Email    | `user@nextmail.com` |
| Password | `123456`           |

After a successful login, you are taken to the **Dashboard** home page.

## Signing out

1. In the left sidebar, click **Sign Out** at the bottom.
2. You return to the login page.

---

## Navigation

The sidebar on the left has three main sections:

| Link        | Purpose                                      |
|-------------|----------------------------------------------|
| **Home**    | Overview with summary stats and charts       |
| **Invoices**| View, search, create, edit, and delete invoices |
| **Customers** | View and search customers with invoice totals |

Click any link to switch pages. The active page is highlighted in blue.

---

## Dashboard (Home)

The home page shows an at-a-glance view of your business:

- **Collected** — total amount from paid invoices
- **Pending** — total amount from unpaid invoices
- **Total Invoices** — number of invoices in the system
- **Total Customers** — number of customers
- **Recent Revenue** — bar chart of monthly revenue
- **Latest Invoices** — five most recent invoices with customer name and amount

Data loads automatically when you open the page.

---

## Invoices

### View invoices

1. Click **Invoices** in the sidebar.
2. The table lists each invoice with:
   - Customer name and photo
   - Email
   - Amount
   - Date
   - Status (**Pending** or **Paid**)
   - Edit and Delete actions

### Search invoices

1. Type in the **Search invoices...** box at the top.
2. Results filter as you type (by customer name, email, amount, date, or status).
3. Clear the search box to see all invoices again.

### Pagination

- If there are many invoices, use the page numbers at the bottom to move between pages.

### Create an invoice

1. Click **Create Invoice** (top right).
2. Fill in:
   - **Customer** — select from the dropdown
   - **Amount** — invoice amount in dollars
   - **Status** — Pending or Paid
3. Click **Create Invoice**.
4. You are returned to the invoices list.

### Edit an invoice

1. On the invoices table, click the **pencil** icon on the row you want to change.
2. Update customer, amount, or status.
3. Click **Edit Invoice**.

If the invoice ID is invalid, you see a “Not Found” page.

### Delete an invoice

1. Click the **trash** icon on the invoice row.
2. Confirm deletion in the dialog.
3. The invoice is removed from the list.

---

## Customers

### View customers

1. Click **Customers** in the sidebar.
2. The table shows:
   - Name and profile photo
   - Email
   - Total Invoices
   - Total Pending (amount)
   - Total Paid (amount)

### Search customers

1. Type in the **Search customers...** box.
2. Results filter by customer name or email.

---

## Tips

- **Refresh the page** if data looks outdated after creating or editing records.
- **Stay logged in** — protected pages require an active session; bookmark the login page if needed.
- **Mobile** — the layout adapts to smaller screens; invoice and customer tables use a card layout on mobile.

---

## Getting help

- For setup, deployment, or developer details, see [DOCUMENTATION.md](./DOCUMENTATION.md).
- This app was built following the [Next.js App Router Course](https://nextjs.org/learn/dashboard-app).
