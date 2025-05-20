# AI Rules Repository

This repository serves as a centralized collection of AI rules, guidelines, and knowledge bases specifically tailored for AI coding tools and custom assistants. It's designed to enhance collaboration between developers and AI systems based on different technology stacks.

## Purpose

The main purpose of this repository is to:

- Store and organize AI-specific rules and guidelines
- Maintain knowledge bases for different technology stacks
- Provide consistent interaction patterns with AI coding assistants
- Enable better collaboration between developers and AI tools
- Share best practices for AI-assisted development

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

Contributions are welcome! Please feel free to submit pull requests with:

- New rules or guidelines
- Updated knowledge bases
- Improved templates
- Better documentation
- Bug fixes

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

[Add your preferred contact method here]
