## fabric-patterns

My personal collection of custom [Fabric](https://github.com/danielmiessler/fabric) patterns.

## Patterns

| Pattern        | Description                                                                                                               |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `note_youtube` | Transforms a YouTube video transcript into a structured Obsidian note with summary, key takeaways, quotes, and references |

## Installation

Clone the repo and symlink or copy the patterns into your Fabric config directory:

```bash
git clone https://github.com/yourusername/fabric-patterns.git
```

**Option A — Copy patterns (simple):**

```bash
cp -r fabric-patterns/note_youtube ~/.config/fabric/patterns/
```

**Option B — Symlink patterns (stays in sync with repo):**

```bash
ln -s ~/fabric-patterns/note_youtube ~/.config/fabric/patterns/note_youtube
```

Verify Fabric can see the pattern:

```bash
fabric --list | grep note_youtube
```

## Usage

Run a pattern directly:

```bash
fabric -y "https://youtube.com/watch?v=..." --pattern note_youtube
```

Or if you have the Obsidian alias setup in your `.bashrc` (see below), pipe into the pattern and pass a title to save directly to your vault:

```bash
fabric -y "https://youtube.com/watch?v=..." | note_youtube "your-note-title"
```

## Obsidian Vault Integration

To save pattern output directly to an Obsidian vault, add the following to your `.bashrc` or `.zshrc`. This dynamically creates a shell function for every pattern in your Fabric config directory:

```bash
# Obsidian vault path
obsidian_base="/path/to/your/obsidian/vault"

# Create a shell function for each Fabric pattern
for pattern_file in ~/.config/fabric/patterns/*; do
    pattern_name=$(basename "$pattern_file")
    unalias "$pattern_name" 2>/dev/null
    eval "
    $pattern_name() {
        local title=\$1
        local date_stamp=\$(date +'%d-%m-%Y')
        local output_path=\"\$obsidian_base/\${date_stamp}-\${title}.md\"
        if [ -n \"\$title\" ]; then
            fabric --pattern \"$pattern_name\" > \"\$output_path\"
        else
            fabric --pattern \"$pattern_name\" --stream
        fi
    }
    "
done
```

After editing, reload your shell:

```bash
source ~/.bashrc
```

Notes are saved as `DD-MM-YYYY-title.md` in the vault root. Move them to subfolders manually or adjust `output_path` to fit your vault structure.

## Adding a New Pattern

1. Create a new directory under the repo root with your pattern name (use lowercase with underscores):

```bash
mkdir my_pattern_name
```

2. Create a `system.md` file inside it. Follow this structure:

```markdown
# IDENTITY and PURPOSE

Describe what the AI is and what it does.

---

# STEPS

1. Step one
2. Step two
   ...

---

# OUTPUT STRUCTURE

Describe the exact output format with an example.

---

# OUTPUT INSTRUCTIONS

- Specific rules and constraints for the output.
- ...

Ensure you follow ALL of these instructions.
```

3. Optionally add a `user.md` with a short human-readable description of the pattern — useful if you plan to share or contribute it upstream.

4. Symlink or copy the new pattern into `~/.config/fabric/patterns/` as described in the installation steps.

## Resources

- [Fabric GitHub](https://github.com/danielmiessler/fabric)
- [Fabric Pattern Reference](https://github.com/danielmiessler/fabric/tree/main/data/patterns)
- [Fabric Documentation](https://deepwiki.com/danielmiessler/fabric) my_fabric_patterns

