# Contributing to BUMM

Thank you for your interest in contributing to BUMM! This document provides guidelines for contributing to our AI-powered Solana smart contract builder.

## 🚀 Getting Started

### Prerequisites
- Node.js 18+
- Python 3.11+
- Solana CLI
- Git

### Development Setup

1. **Fork the repository**
2. **Clone your fork**
   ```bash
   git clone https://github.com/bumm-ai/bumm.git
   cd bumm
   ```

3. **Set up the development environment**
   ```bash
   # Frontend
   cd frontend
   npm install
   
   # Backend
   cd ../../bumm-api
   pip install -r requirements.txt
   
   # AI Agent
   cd ../ai-audit-agent
   pip install -r requirements.txt
   ```

## 📝 Development Guidelines

### Code Style
- **Frontend**: Follow ESLint and Prettier configurations
- **Backend**: Follow PEP 8 and use Black formatter
- **AI Agent**: Follow Python best practices

### Commit Messages
Use conventional commit format:
```
feat: add new contract generation feature
fix: resolve wallet connection issue
docs: update API documentation
test: add unit tests for security agent
```

### Branch Naming
- `feature/description` - New features
- `fix/description` - Bug fixes
- `docs/description` - Documentation updates
- `refactor/description` - Code refactoring

## 🧪 Testing

### Frontend Testing
```bash
cd frontend
npm run test
npm run test:e2e
```

### Backend Testing
```bash
cd bumm-api
pytest tests/
```

## 📋 Pull Request Process

1. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Write clean, well-documented code
   - Add tests for new functionality
   - Update documentation as needed

3. **Test your changes**
   - Run all tests
   - Test manually in development environment
   - Ensure no breaking changes

4. **Submit a pull request**
   - Provide clear description of changes
   - Link any related issues
   - Request review from maintainers

## 🐛 Reporting Issues

### Bug Reports
When reporting bugs, please include:
- **Environment**: OS, Node.js version, Python version
- **Steps to reproduce**: Clear, step-by-step instructions
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Screenshots**: If applicable

### Feature Requests
For feature requests, please include:
- **Use case**: Why is this feature needed?
- **Proposed solution**: How should it work?
- **Alternatives**: Other solutions considered

## 🔒 Security

### Reporting Security Issues
- **DO NOT** create public issues for security vulnerabilities
- Email security issues to: support@bumm.io
- Include detailed information about the vulnerability
- Allow reasonable time for response before disclosure

## 📚 Documentation

### Code Documentation
- Document all public APIs
- Include type hints in Python code
- Add JSDoc comments for complex JavaScript functions
- Update README files when adding new features

### User Documentation
- Update user guides for new features
- Add screenshots for UI changes
- Keep API documentation current

## 🎯 Areas for Contribution

### High Priority
- **Security Enhancements**: Improve AI security auditing
- **Performance Optimization**: Speed up contract generation
- **UI/UX Improvements**: Enhance user experience
- **Testing Coverage**: Increase test coverage

### Medium Priority
- **New Features**: Additional contract templates
- **Integration**: More wallet providers
- **Documentation**: Improve guides and examples
- **Accessibility**: WCAG compliance improvements

### Low Priority
- **Code Refactoring**: Clean up legacy code
- **Dependencies**: Update and optimize dependencies
- **CI/CD**: Improve automation
- **Monitoring**: Add more metrics and logging

## 🏆 Recognition

Contributors will be recognized in:
- **README**: Listed as contributors
- **Release Notes**: Mentioned in relevant releases
- **Documentation**: Credited for significant contributions

## 📞 Getting Help

- **Discord**: Join our community Discord
- **GitHub Discussions**: Use GitHub Discussions for questions
- **Email**: Contact us at support@bumm.io

## 📄 License

By contributing to BUMM, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to BUMM! 🚀
