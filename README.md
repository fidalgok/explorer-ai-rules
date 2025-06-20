# AI Rules Repository

This is my personal collection of AI rules, guidelines, and knowledge bases specifically tailored for AI coding tools and custom assistants. It's designed to enhance my workflow when using AI systems with different technology stacks, but I'm making it public in case others find it useful.

## Purpose

The main purpose of this repository is to:

- Store and organize AI-specific rules and guidelines for my projects
- Maintain knowledge bases for different technology stacks I work with
- Provide consistent interaction patterns with AI coding assistants
- Enable better collaboration between myself and AI tools
- Document best practices for AI-assisted development that work for me

## Repository Structure

```bash
.
├── rules/                   # AI rules and guidelines
│   ├── project-tech/        # Technology-specific rules
│   │   ├── cloudflare/      # Cloudflare Workers, Pages, D1, KV, R2
│   │   ├── convex/          # Convex backend platform
│   │   ├── feature-development/ # Feature development guidelines
│   │   ├── firebase/        # Firebase services
│   │   ├── llms/            # LLM-related rules
│   │   ├── netlify/         # Netlify deployment and functions
│   │   ├── react-router-v7/ # React Router v7 specific rules
│   │   ├── shadcn/          # shadcn UI component library
│   │   └── tailwind/        # Tailwind CSS framework
│   └── user/                # User-specific rules and preferences
│       ├── coding-assistant.md      # Core programming principles
│       └── security-best-practices.md # Security guidelines for AI assistants
└── CLAUDE.md                # Instructions for Claude Code
```

## Usage

### Option 1: Git Submodule Integration (Recommended)

To integrate these AI rules into your existing projects:

1. **Add as Submodule**

   ```bash
   # In your project root directory
   git submodule add https://github.com/fidalgok/explorer-ai-rules.git .ai-rules
   git commit -m "Add AI rules submodule"
   ```

2. **Reference in Your Project's CLAUDE.md**

   ```markdown
   # AI Rules Integration

   This project includes comprehensive AI coding guidelines from [.ai-rules/](./.ai-rules/)

   For technology-specific rules, see:

   - [Cloudflare Guidelines](./.ai-rules/rules/project-tech/cloudflare/)
   - [React Router Guidelines](./.ai-rules/rules/project-tech/react-router-v7/)
   - [Security Best Practices](./.ai-rules/rules/user/security-best-practices.md)
   ```

3. **Update Rules in Any Project**

   ```bash
   # Pull latest AI rules updates
   git submodule update --remote .ai-rules
   git add .ai-rules
   git commit -m "Update AI rules to latest version"
   ```

4. **Clone Projects with Submodules**

   ```bash
   # When cloning projects that use this submodule
   git clone --recurse-submodules <your-project-repo>

   # Or if already cloned
   git submodule init
   git submodule update
   ```

5. **Contributing Changes Back from Submodule**

   When working in a project and you need to update AI rules based on your work:

   ```bash
   # Navigate to the submodule directory
   cd .ai-rules

   # Make your changes to the AI rules files
   # Edit rules/project-tech/your-framework/your-file.md

   # Commit changes in the submodule
   git add .
   git commit -m "Update AI rules based on project work"

   # Push to the AI rules repository
   git push origin main

   # Go back to parent project and update submodule reference
   cd ..
   git add .ai-rules
   git commit -m "Update AI rules submodule reference"
   ```

   This workflow allows you to evolve AI rules based on real project experience while keeping them centralized for use across all your projects.

### Managing This Repository

1. **Adding New Rules**

   - Create new rule files in the appropriate directory under `/rules/project-tech/`
   - Follow kebab-case naming conventions
   - Include clear documentation and examples

2. **Technology Stack Organization**

   - Each technology has its own directory under `/rules/project-tech/`
   - Include llms.txt references for up-to-date documentation
   - Keep rules specific to each technology stack

3. **User Rules**
   - Store personal preferences under `/rules/user/`
   - Include coding style preferences, security guidelines, etc.

## Contributing

While this is primarily a personal repository that I maintain for my own use, I appreciate suggestions and ideas. If you find this useful and have improvements to suggest:

- Feel free to fork the repository for your own use
- Open an issue if you find errors or have suggestions
- Submit pull requests if you'd like to contribute improvements

## License

MIT License

Copyright (c) 2024

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

## Contact

This is a personal project maintained by [fidalgok](https://github.com/fidalgok). If you have questions or suggestions, please open an issue in this repository.
