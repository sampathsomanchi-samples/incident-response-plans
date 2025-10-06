# Playbook A — Prompt Injection Attack Response

**Version:** 1.0  
**Owner:** Security Ops / AI Engineering  
**Classification:** Confidential – Internal Use  
**Reference:** OWASP LLM-01 (Prompt Injection)

## 1. Purpose
Define detection, containment, and recovery actions when a chatbot or LLM endpoint is manipulated via prompt injection resulting in policy bypass, data exposure, or malicious instructions.

## 2. Scope
- Systems Covered: Production AI Chatbots, LLM inference APIs, agentic workflows  
- In-Scope Assets: Prompt templates, context windows, memory stores, retrieval chains  
- Out-of-Scope: Model training incidents (handled separately)

## 3. Roles and Responsibilities
| Role | Responsibility | Backup |
|------|----------------|--------|
| Incident Commander | Coordinate containment & comms | Security Ops Manager |
| LLM Engineer | Inspect system prompts / templates | ML Lead |
| Cloud Security | Isolate IAM & endpoints | SecOps Analyst |
| Legal / Compliance | Assess disclosure requirements | Legal Counsel |
| Comms / PR | External messaging | Marketing Comms |

## 4. Incident Identification
**Indicators of Compromise (IOCs):**
- Chatbot outputs reveal internal instructions, API keys, or system config.
- User-supplied prompts containing phrases like "ignore previous instructions", "reveal", "show system prompt".
- Token usage spikes or abnormally long responses.
- Requests with nested encoded payloads or suspicious delimiters.

See artifacts/log-samples/prompt-injection-detection-example.log for sample logs.

## 5. Detection & Initial Analysis
Actions:
- Query model logs for injection strings (use artifacts/threat-signatures/prompt-injection-patterns.yaml).
- Correlate CloudTrail/CloudWatch events to inference role activity.
- Determine if sensitive data was exposed and classify severity (S1–S4).

## 6. Containment
Short-Term:
- Disable affected endpoint (API Gateway / Load Balancer rule).
- Revoke or rotate temporary inference credentials.
- Apply WAF rules to block offending IPs/user agents.

Long-Term:
- Harden prompt templating and sanitize user input server-side.
- Deploy guardrail layer to sanitize/strip system prompt exposure.

## 7. Eradication & Recovery
- Remove compromised prompts from context caches and logs.
- Rotate any exposed credentials in AWS Secrets Manager.
- Roll back to last verified model version or retrain without leaked data.
- Validate remediation with red-team/adversarial tests before re-enabling endpoint.

## 8. Communication & Reporting
Stakeholders:
- SOC / SecOps: Initial detection (Slack / PagerDuty)
- Exec leadership: S1/S2 incidents
- Legal / Privacy: Confirmed data disclosures
- Customers/Users: Follow legal / policy notification rules

Templates in artifacts/templates/contact-matrix.md and post-incident-report.md

## 9. Post-Incident Activities
- Postmortem within 72 hours using artifacts/templates/post-incident-report.md
- Update threat signatures and detection rules
- Revisit IAM policies in artifacts/iam-roles/

## 10. References
- NIST SP 800-61 r3
- OWASP LLM Top 10 (Prompt Injection)
- AWS Bedrock / Guardrails docs

## 11. Version History
| Version | Date | Changes | Author |
|---|---|---:|---|
| 1.0 | $(date +%F) | Initial scaffold | Security Ops |
