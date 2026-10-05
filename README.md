# Personal Learning Notes

## About this repository

This repository is a personal collection of practical learning notes, command references, troubleshooting records, and examples. The notes reflect topics explored through hands-on work and study; they are organized by subject so they can be used as a reference or read as a learning path.

## What I am learning

The notes cover system administration, networking, Drupal and web development, collaboration administration, and working with hardware and devices. Some pages are concise command cheat sheets; others record investigations, setup steps, or interview preparation.

## Topics covered

- **Networking:** [BGP](BGP%20Review.md), [VLAN 2002 device tracing](vlan2002_summary.md), and an [Aruba switch cheat sheet](ArubaCheatSheet.md)
- **Linux and server administration:** [Postfix commands](Postfix-Commands.md), [why Postfix sent old mail](Understanding%20Why%20Postfix%20Sent%20Old%20Emails.md), [PHP-FPM](php-fpm.md), [Apache custom 403 pages](Add%20403%20Error%20in%20Apache.md), and [removing Thunderbird](How%20to%20Permanently%20Remove%20Thunderbird%20Mail%20from%20Linux.md)
- **Drupal and web development:** [Drupal interview scenarios](drupalinterview.md), [Drupal 8.9 to 9.5 upgrade notes](Drupal8.9to9.5.md), [Git and Drupal workflow](GitBasics.md), [CSV imports in Drupal](Importing%20CSV.md), and [SSH scripts in PHP](SSH%20Scripts%20in%20PHP.md)
- **Google Workspace administration:** [GAM commands](Google%20Workspace%20GAM%20commands.md) and the similarly titled [GAM commands reference](gamcommands.md)
- **Other systems and hardware:** [hypervisors and ESXi](What%20is%20Esxi.md), [tape library operations](TapeLibrary.md), [Realme X7 Pro and ADB](RealmeX7Bloatware.md), and a [WordPress site report](WordpressReport.md)

## Repository structure

All notes currently live in the repository root. Filenames are kept descriptive, with Markdown links in this page providing a subject-based index.

```text
.
├── README.md
├── Networking notes
├── Linux and server administration notes
├── Drupal and web development notes
├── Google Workspace / GAM references
└── Hardware, device, and platform notes
```

The labels in this outline describe topic groups; they are not directories.

## Learning notes and navigation

For a suggested route through related notes:

1. Start with [Git basics and a Drupal project workflow](GitBasics.md).
2. Review the [Drupal upgrade notes](Drupal8.9to9.5.md), [CSV import lessons](Importing%20CSV.md), and [interview scenarios](drupalinterview.md).
3. Explore server-side topics such as [PHP-FPM](php-fpm.md), [Apache error pages](Add%20403%20Error%20in%20Apache.md), and [Postfix troubleshooting](Postfix-Commands.md).
4. Continue with the [networking command references](ArubaCheatSheet.md), [VLAN device tracing](vlan2002_summary.md), and [BGP review](BGP%20Review.md).

The two GAM pages contain substantially overlapping command examples. Use either [Google Workspace GAM commands](Google%20Workspace%20GAM%20commands.md) or [GAM commands](gamcommands.md); the latter also includes its source links.

## How to use these notes

- Browse by topic using the links above, or open a Markdown file directly.
- Use the command examples as a starting point and adapt placeholders such as `<IP>`, `<interface>`, and `user@domain.com` to your environment.
- Follow links inside each note for related material and any resources already cited there.
- Treat setup and administration commands as notes from particular learning contexts; check that paths, versions, and system details match your environment before using them.

## Useful resources

Resources cited within the notes are retained in their relevant pages. Examples include the [GAM Wiki](https://github.com/GAM-team/GAM/wiki?utm_source=chatgpt.com) in the GAM references, Drupal documentation links in the upgrade and SSH notes, and virtualization references in [What is ESXi?](What%20is%20Esxi.md).

## Progress and learning roadmap

The collection suggests a path from foundational workflows (Git and Drupal project structure), through application and server troubleshooting, to networking and infrastructure operations. This is an organizational guide inferred from the existing notes, not a claim that every topic has been completed or that the notes follow a dated curriculum.

## Personal notes

This is a personal learning repository. Its notes are maintained for reference and may include environment-specific examples. No contribution or contact process is defined here.
