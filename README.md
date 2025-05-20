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
├── .github/                 # GitHub configuration files
│   └── workflows/           # GitHub Actions workflows
│       └── markdownlint.yml # Markdown linting workflow
├── rules/                   # AI rules and guidelines
│   ├── project-tech/             # Project-specific rules
│   │   ├── cloudflare/      # Cloudflare specific rules
│   │   ├── feature-development/ # Feature development guidelines
│   │   ├── firebase/        # Firebase specific rules
│   │   ├── llms/            # LLM-related rules
│   │   ├── react-router-v7/ # React Router v7 specific rules
│   │   ├── shadcn/          # shadcn UI specific rules
│   │   └── tailwind/        # Tailwind CSS specific rules
│   └── user/                # User-specific rules and preferences
└── .markdownlint.yaml       # Markdown linting configuration
```

## Usage

1. **Adding New Rules**

   - Create new rule files in the appropriate directory under `/rules/project/$tech`
   - Follow the established naming conventions
   - Include clear documentation and examples

2. **Technology Stack Organization**

   - Each technology stack has its own directory under `/rules/project/$tech`
   - Examples include:
     - Framework rules (Next.js, React Router)
     - Styling rules (Tailwind CSS, shadcn)
     - Backend/Database rules (Firebase, Supabase, Turso with Drizzle)
   - Keep rules specific to each technology stack in their respective directories

3. **User Rules**
   - Store personal preferences and custom rules under `/rules/user`
   - These can include coding style preferences, naming conventions, etc.

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
