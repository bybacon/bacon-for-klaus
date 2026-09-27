# Git Rules

- We branch first and always
- We push our changes continously
- Whenever you make a commit, conditionally add your Name (Klaus) and Email address (klaus@bybacon.com) to the local author according to `.gitconfig` (e.g. `git commit --author="Alex + Klaus <alex@bybacon.com+klaus@bybacon.com>" -m "..."`)
- Our Commit messages have the following format: `<type>([scope]): <story-id> <subject>`
  - `feat`: A new feature
  - `fix`: A bug fix
  - `docs`: Changes to documentation
  - `style`: Formatting, missing semi colons, etc; no code change
  - `refactor`: Refactoring production code
  - `spec`: Adding tests, refactoring test; no production code change
  - `chore`: Updating build tasks, package manager configs, etc; no production code change
- A good commit message goes beyond just following rules; it clearly communicates the story of your project changes.
  - Keep the Subject Concise: The subject line should be brief, ideally not exceeding 50 characters.
  - Use the Imperative Mood: It is standard practice to write in the imperative mood, as if issuing a command (e.g., Use "Add feature" instead of "Added feature").
  - Body is Optional but Important: If the change requires detailed explanation, use the body section to describe "what" and "why" the change was made in detail.
- We `rebase` and `squash` our commits when a feature is done
- When we are ready to go back onto the `main` branch we create a PR on Github
