# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a **DevOps Bootcamp training repository** containing hands-on lab exercises in Malay language (Bahasa Melayu). The repository is structured as a learning resource for participants to practice Git workflows, AWS cloud infrastructure, and Linux fundamentals.

## Repository Purpose

- **Educational focus**: Teaching DevOps fundamentals through practical labs
- **Language**: All documentation and instructions are in Malay (Bahasa Melayu)
- **Audience**: Bootcamp participants (peserta) learning DevOps practices
- **Workflow**: Fork-based contribution model where participants submit their biodata via Pull Requests

## Directory Structure

```
bootcamp-devops/
├── labs/           # Lab exercise files (lab1.md - lab21.md)
├── peserta/        # Participant biodata files (one .md file per participant)
└── assets/         # Images and diagrams for lab instructions
```

### Key Directories

- **`labs/`**: Contains numbered lab exercises (lab1.md through lab21.md) organized by session topics
- **`peserta/`**: Contains participant biodata markdown files following the naming convention `[nama-peserta].md` (lowercase, hyphenated)
- **`assets/`**: Contains supporting images (vpc.png, igw.png, rt.png, subnets.png) referenced in lab materials

## Lab Organization

Labs are organized into sessions covering different topics:

### Sesi 4: GitHub (Labs 1-4)
- **Lab 1**: SSH key setup for GitHub authentication
- **Lab 2**: Forking and cloning the bootcamp repository
- **Lab 3**: Creating feature branches and adding participant biodata
- **Lab 4**: Pushing branches and creating Pull Requests

### Sesi 6: AWS Basics (Labs 5-10)
- **Lab 5**: Creating AWS accounts
- **Lab 6**: Launching EC2 instances
- **Lab 7**: Connecting via SSH
- **Lab 8**: Installing Nginx
- **Lab 9**: Testing from browser
- **Lab 10**: Cleanup procedures

### Sesi 7: AWS Networking (Labs 11-14)
- **Lab 11**: VPC creation and configuration
- **Lab 12**: Internet Gateway setup
- **Lab 13**: Route Table configuration
- **Lab 14**: Subnet management

### Sesi 8: AWS Security (Labs 15-17)
- **Lab 15**: IAM Roles and Security Groups
- **Lab 16**: SSM Session Manager
- **Lab 17**: Network Communication

### Sesi 7+8: Advanced Networking (Labs 18-19, Optional)
- **Lab 18**: NAT Gateway (has cost implications - requires cleanup)
- **Lab 19**: VPC Endpoints (cost-effective alternative for AWS services)

### Sesi 11-14: Linux Basics (Labs 20-21)
- **Lab 20**: File navigation (`pwd`, `ls`, `cd`, `mkdir`, `rmdir`, permissions)
- **Lab 21**: File operations

## Important Naming Conventions

### Participant Biodata Files
- **Format**: `[nama-peserta].md` (lowercase, hyphenated for spaces)
- **Examples**:
  - Ahmad Bin Ali → `ahmad-ali.md`
  - Siti Nurhaliza → `siti-nurhaliza.md`
- **Location**: `peserta/` directory

### Branch Naming
- **Format**: `add-[nama-peserta]` (lowercase, no spaces)
- **Example**: `add-ahmad` for participant named Ahmad

### Commit Messages
- **Format**: `docs: add biodata for [Nama Lengkap]`
- **Example**: `docs: add biodata for Ahmad Abdullah`

### Pull Request Titles
- **Format**: `docs: add peserta biodata - [Nama Lengkap]`
- **Example**: `docs: add peserta biodata - Ahmad Abdullah`

## Biodata Template

Participant biodata files follow this structure:

```markdown
# Biodata Peserta

## Maklumat Peribadi
- **Nama Penuh:** [Full Name]

## Pekerjaan
- **Jawatan Semasa:** [Current Job Title]

## Hobi
- 🎯 [Hobby]

## Fun Fact
> [Interesting fact about the participant]

## Hubungi Saya
- 📧 Email: [email - optional]
- 🔗 LinkedIn: [LinkedIn profile - optional]
- 🐙 GitHub: [GitHub username]
```

## Git Workflow

The repository uses a **fork-and-pull-request workflow**:

1. Participants fork `opariffazman/bootcamp-devops` to their own GitHub account
2. Clone their fork locally
3. Create a feature branch following the naming convention
4. Add their biodata file to the `peserta/` directory
5. Commit and push to their fork
6. Submit a Pull Request from their fork to the main repository

**Critical PR Settings**:
- **Base repository**: `opariffazman/bootcamp-devops`
- **Base branch**: `main`
- **Head repository**: Participant's fork
- **Compare branch**: `add-[nama-peserta]`

## Working with Lab Files

### When Adding New Labs
- Number sequentially (lab22.md, lab23.md, etc.)
- Update README.md with the new lab link and description
- Follow the existing lab structure with clear steps and examples
- Include prerequisites section if the lab depends on previous labs
- Add visual diagrams to `assets/` directory if needed

### When Modifying Existing Labs
- Maintain the Malay language throughout
- Keep the step-by-step instructional format
- Include code examples with expected outputs
- Add troubleshooting sections where relevant
- Preserve the checklist at the end of each lab

## Language and Style Guidelines

- **Primary language**: Malay (Bahasa Melayu)
- **Technical terms**: Often kept in English (e.g., "Pull Request", "SSH key", "VPC")
- **Tone**: Educational, clear, beginner-friendly
- **Code formatting**: Use backticks for commands, triple backticks for code blocks
- **Explanations**: Include "Expected output" and "Penjelasan" (explanation) sections

## Important Notes

- **Cost awareness**: Lab 18 (NAT Gateway) includes explicit cost warnings and cleanup reminders
- **SSH vs SSM**: Later labs use SSM Session Manager instead of direct SSH for EC2 access
- **Nginx context**: Linux labs (20-21) assume Nginx is installed and use it for practical examples
- **Prerequisites**: Each advanced lab references previous labs for setup requirements
