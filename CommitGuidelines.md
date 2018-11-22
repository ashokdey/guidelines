# Commit Guidelines

So why you need commit guidelines at all? If you browse the log of any random Git repository, you will probably find its commit messages are more or less a mess.

So I started follwoing a pattern that try to clear the mess a little less messy. My commit message has the following pattern:
`--[verb]:[changes performed]`

Verb can be any of the following:

- `init`
- `update`
- `fix`
- `refactor`
- `log`
- `removed`

## Sample commit messages

- First commit after settin-up a repo: `--init: initial setup`
- After adding new changes: `--update: added db connections`
- After making any fix: `--fix: DB pool connection instetad of normal connection`
- After removing anything: `--removed: redis was no longer required for todo app`
- When you moved a repeting code-block as util function: `--refactor: new util to validate array payload for todos`
- After adding logs for debug: `--log: added debudding logs inside update todo controller`

## Example repos:

- Almost all my recent repo follow this commit convention. Checkout [my repos](https://github.com/ashokdey?tab=repositories)
- Recently **[Gaurav](https://github.com/igauravsehrawat)** appreciated and adopted this convention. Checkout [his payroll system demo app](https://github.com/igauravsehrawat/payroll-system)
