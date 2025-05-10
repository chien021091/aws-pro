AWS CodeGuru Reviewer
- An automated code review tool that analuze source code for
    - Security vulnabilities (hardcode secret, insecure API usage)
    - Code quality issues
    - Best practices
- Integrated with CodeCommit to scan repository on pull request or commits
- Provides recommandations, including specific issues like harcode credentials, with actionable advice
- Support Python and SAM Template

AWS CodeGuru profiler
- A performance profiling tool for optimizing application performance
- Analyzes runtime behavior of lambda function using decorators like @with_lambda_profiler()
- Not designed for static code analysys or detecting hardcoded secret in repository