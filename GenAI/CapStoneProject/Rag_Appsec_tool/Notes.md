```
pip install langchain langchain-community langchain-ollama chromadb faiss-cpu sentence-transformers tiktoken pypdf jupyter ipykernel
```



Generate detailed cybersecurity knowledge covering all OWASP Top 10 vulnerabilities. For each vulnerability include: 1) Description 2) How to detect it in code 3) Vulnerable code example in Python and JavaScript 4) Secure code example 5) CVSS risk level 6) Remediation steps. Cover: SQL Injection, XSS, Broken Authentication, IDOR, Security Misconfiguration, Cryptographic Failures, Injection flaws, Insecure Deserii alization, Using Components with Known Vulnerabilities, Insufficient Logging. Be detailed and technical.

```
python -m ipykernel install --user --name=owasp-rag --display-name "Python (owasp-rag)" 

pip uninstall sentence-transformers torch torchvision torchaudio -y
```


A local AI-powered security tool that automatically detects OWASP Top 10 vulnerabilities in any code snippet and explains exactly how to fix them — built for AppSec consultants to speed up code review during penetration testing engagements, without sending client code to any external API or cloud service.
