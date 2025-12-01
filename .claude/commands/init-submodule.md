# Initialize Submodule Context

You are helping the user initialize their working context for a specific git submodule in this repository.

## Your Task

1. **List available submodules** by running:
   ```bash
   git submodule status
   ```

2. **Ask the user which submodule they want to work on**
   Present the list and ask them to choose.

3. **Once chosen, load the submodule's initialization file**:
   - Read the file: `<submodule-path>/AMPLIFIER_INIT.md`
   - Display its contents to provide context about that submodule

4. **Create the initialization marker**:
   - Create file: `.claude/.amplifier-submodule-init`
   - Content should be: `<submodule-name>`
   - This prevents the prompt from showing again

5. **Confirm**:
   - Tell the user they're now set up to work on the chosen submodule
   - Mention they can run `/init-submodule` again if they want to switch submodules

## Important Notes

- If `.claude/.amplifier-submodule-init` already exists, first show what's currently initialized
- Ask if they want to keep it or change to a different submodule
- The AMPLIFIER_INIT.md file should contain project-specific context for that submodule
