# First Project

This walkthrough takes you from an empty folder to a running PHPShift project with your first AI-generated page.

## Step 1 - Create a New Project

Open a terminal in your desired project directory and run:

```bash
phpshift new
```

PHPShift will prompt you for project details. These values are injected into your project's `.env` and initial files:

| Prompt | Example |
|--------|---------|
| Project name | `My App` |
| Project author | `John Doe` |
| Local domain | `myapp.local` |
| Production domain | `myapp.com` |
| Database host | `localhost` |
| Database name | `myapp_db` |
| Database user | `root` |
| Database password | *(your MySQL root password)* |
| AI model | *(select from list)* |
| AI key / key file | *(your API key)* |

After entering these, PHPShift:

1. Copies the project frame files into your directory
2. Generates a secure random `APP_KEY`
3. Asks if you want to initialize Git
4. Asks if you want to refine your project idea (AI generates a `.md` plan)
5. Installs a LICENSE file
6. Creates a VS Code workspace shortcut on your Desktop
7. Opens `code .md` so you can review your project plan

## Step 2 - Refine Your Idea (Optional)

When prompted "Want to refine your idea?", type a short description of your project:

```
A task management app where users can create, assign and track tasks in teams
```

The AI generates a structured `.md` project plan. Review it in VS Code. If you like it, confirm - the plan is saved and will guide future AI generations. If not, it's rolled back.

## Step 3 - Start Development

When asked "Want to start development?", answer **Yes**. Or start later with:

```bash
phpshift start
```

PHPShift will:
- Configure and start XAMPP (Apache + MySQL)
- Add your local domain to the hosts file and Apache virtual hosts
- Connect to the database (create it if needed)
- Open your project in the browser
- Launch the interactive skill menu

## Step 4 - Create Your First Page

In the skill menu, select **Create → Page**, then describe your page:

```
A public landing page with a hero section, feature highlights, and a call-to-action button that links to registration
```

PHPShift collects project context (schema, PHP methods, JS methods) and sends it to the AI. In a few seconds, it writes:

- `Pages/public.welcome/readme.md`
- `Pages/public.welcome/page.html`
- `Pages/public.welcome/style.css`
- `Pages/public.welcome/script.js`
- `Pages/public.welcome/code.php`

If a database table is needed, `db.sql` appears in VS Code for your review before being applied.

## Step 5 - Review and Confirm

The CLI asks: **"Want to keep changes?"**

- **Yes** - changes are confirmed and you're offered a Git commit
- **Redo** - changes roll back; describe your task again with adjustments
- **No** - changes roll back; menu returns

## Step 6 - View Your Page

Open your browser at `http://myapp.local/welcome` and see the generated page live. Iterate by selecting **Edit → Page** to refine it.

## Stopping the Project

When done for the day:

```bash
phpshift stop
```

Or select the exit option in the skill menu. PHPShift removes the virtual host, updates the hosts file, and stops XAMPP (if no other projects are running).
