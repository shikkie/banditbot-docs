# BanditBot Documentation

Welcome to the comprehensive documentation for BanditBot - a sophisticated Discord moderation and utility bot.

## 📚 Documentation Sections

### 📄 [Software Requirements](REQUIREMENTS.md)
System requirements specification with functional and non-functional requirements.

### 🎯 [User Guide](USER_GUIDE.md)
Complete guide for end users, including command usage, moderation features, and troubleshooting.

### 📋 [Commands Reference](COMMANDS.md)
Detailed documentation of all available commands, triggers, and action flows.

### ⚡ [Slash Commands](SLASH_COMMANDS.md)
Discord slash commands configuration, including mapped and standalone commands.

### 🔥 [Regex Triggers Guide](TRIGGERS.md)
Comprehensive guide to regex triggers, pattern groups, and match group capture with practical examples.

### 🌐 [API Documentation](API.md)
REST API reference for configuration management and data access.

### 🎬 [Action System](ACTIONS.md)
Technical documentation for the action system and creating custom actions.

### 🧪 [Manual Testing Guide](MANUAL_TESTING_GUIDE.md)
Comprehensive guide for manual Discord testing that cannot be automated.

### 🔧 [Automated Testing Guide](../TESTING.md)
Complete documentation for the automated test suite and CI/CD integration.

### 📊 [Test Implementation Summary](../TEST_SUMMARY.md)
Overview of test suite implementation and coverage details.

### 📂 [Configuration Examples](../examples/)
Example configurations for social media monitoring and advanced features.

### 🔧 [Development Scripts Guide](../development-scripts.md)
Enhanced development environment tools including `run-dev.sh` and `stopall.sh`.

## 🎯 Feature Guides

### 🔊 [Voice Features](../TTS_FEATURE.md)
Complete Text-to-Speech and Speech-to-Text functionality with voice customization.

### 📱 [Social Media Monitoring](../SOCIAL_MEDIA_MONITORING.md)
RSS feeds, YouTube channels, and social media tracking with automated posting.

### 🃏 [Blackjack Game](../BLACKJACK.md)
Interactive card game with betting mechanics and Discord button integration.

### 👁️ [OCR Text Extraction](../OCR_FEATURE.md)
Multi-provider image text extraction using Tesseract, EasyOCR, and PaddleOCR.

### 🔒 [Security Features](../SECURITY.md)
Security scanning, rate limiting, and protection mechanisms.

## ⚡ Quick Start Guides

### 📱 [Social Media Quick Start](../SOCIAL_MEDIA_QUICKSTART.md)
5-minute setup guide for RSS feeds and social media monitoring.

### 🔊 [Voice Features Quick Start](../TTS_QUICKSTART.md)
Quick setup for Text-to-Speech functionality and voice customization.

## 🐳 Deployment

###  [Docker Deployment](../DOCKER_DEPLOYMENT.md)
Production deployment guide using Docker and Docker Compose.

## 🚀 Quick Start

### For Users
1. Join a server with BanditBot
2. Use `?help` to see available commands
3. Try `?joke` for a fun example

### For Administrators
1. Set up the bot using the [Docker Deployment Guide](../DOCKER_DEPLOYMENT.md)
2. Configure commands using the web interface at `http://localhost:3000`
3. Read the [User Guide](USER_GUIDE.md) for feature explanations

### For Developers
1. Review the [Software Requirements](REQUIREMENTS.md) for system specifications
2. Study the [Action System](ACTIONS.md) documentation
3. Check the [API Documentation](API.md) for integration
4. Use the [Commands Reference](COMMANDS.md) for configuration examples

## 🔧 Features

- **Modular Command System** - YAML-driven configuration with 69 registered actions
- **Web-Based Management** - Visual configuration interface with real-time updates
- **Advanced Moderation** - Sticky roles, timed actions, comprehensive logging
- **Slash Commands** - Native Discord slash commands with autocomplete
- **Interactive Games** - Blackjack game with betting and button controls
- **Voice Features** - Text-to-speech and speech-to-text with multiple providers
- **OCR Technology** - Extract text from images using multiple OCR providers
- **Social Media Integration** - Automated RSS, YouTube, and social media monitoring
- **Automatic Triggers** - Regex-based message pattern responses
- **Event Handling** - Discord event processing with configurable actions
- **Database Integration** - MongoDB with automated cleanup and capped collections
- **Hot Reload System** - Live configuration updates without restart
- **Comprehensive Testing** - Automated test suite with manual testing guide
- **Production Ready** - Docker deployment with logging and monitoring

## 📞 Support

- **Documentation Issues**: Open an issue on GitHub
- **Bot Usage**: Contact your server administrators
- **Development**: Check the developer documentation sections

---

*This documentation is automatically generated and updated when changes are made to the bot configuration.*