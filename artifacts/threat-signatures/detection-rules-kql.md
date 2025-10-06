# Example detection queries (KQL / Saved search examples)

// KQL-like pseudocode for SIEM:
logs
| where Message matches regex "(?i)ignore (all )?previous instructions|reveal.*system prompt" 
| extend matched = extract("(?i)(ignore (all )?previous instructions|reveal.*system prompt)", 0, Message)
