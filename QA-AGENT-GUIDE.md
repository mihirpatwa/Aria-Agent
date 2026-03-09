# QA Automation Agent Guide

## Overview

The QA Automation Agent is a new addition to the Aria multi-agent system that automatically tests code changes using Chrome MCP (Model Context Protocol) for real browser testing.

## Features

- 🤖 **Automated Browser Testing**: Uses Chrome MCP for real browser automation
- 🔐 **Test Credential Management**: Built-in test account credentials (erik@mailinator.com / Robert@123)
- 📱 **Responsive Testing**: Tests both desktop and mobile viewports
- 📸 **Visual Evidence**: Captures screenshots and console logs
- 🔄 **Fallback Mode**: Provides manual testing instructions when Chrome MCP is unavailable
- 🧪 **Comprehensive Testing**: Covers UI, functionality, authentication, and performance

## Setup

### 1. Configure Test Credentials

The QA agent comes with default test credentials:

```bash
# Edit .env file (optional - defaults are already set)
QA_TEST_EMAIL="erik@mailinator.com"
QA_TEST_PASSWORD="Robert@123"
```

### 2. Chrome MCP Setup

To enable automated browser testing:

#### Option A: Using Claude MCP (Recommended)

1. Install Chrome MCP server
2. Configure MCP connection in your Claude Code environment
3. Set `QA_ENABLE_CHROME_MCP=true` in `Aria-Agent/.env`

#### Option B: Manual Testing Mode

If Chrome MCP is not available, the agent automatically falls back to providing detailed manual testing instructions.

### 3. Development Server

The QA agent expects the development server to be running at:

```bash
# Default URL
http://localhost:5173

# Can be customized in .env
QA_DEV_SERVER_URL="http://localhost:5173"
```

## Usage

### Basic Usage

```bash
cd Aria-Agent

# Test a specific fix
./run.sh "Test the language selector fix for ticket 114644"

# Full QA automation
./run.sh "Run full QA test on recent changes"

# Manual testing mode (when Chrome MCP unavailable)
./run.sh "Generate manual testing instructions for mobile compatibility fix"
```

### Environment Variables

```bash
# Enable/disable Chrome MCP automation
QA_ENABLE_CHROME_MCP="true"  # or "false" for manual mode only

# Test credentials
QA_TEST_EMAIL="erik@mailinator.com"
QA_TEST_PASSWORD="Robert@123"

# Development server URL
QA_DEV_SERVER_URL="http://localhost:5173"
```

## Test Coverage

### UI Changes

- ✅ Visual rendering on desktop browser
- ✅ Visual rendering on mobile viewport
- ✅ Consistent styling across breakpoints
- ✅ No visual regressions

### Functionality Testing

- ✅ Interactive elements work correctly
- ✅ Form submissions and validations
- ✅ Navigation and routing
- ✅ State management

### Authentication Testing

- ✅ Login functionality with test credentials
- ✅ Session persistence
- ✅ User permissions and access control
- ✅ Logout functionality

### Audio/Video Testing

- ✅ Microphone permission handling
- ✅ Audio playback functionality
- ✅ Video/avatar display
- ✅ WebRTC and WebSocket connections

### Performance Testing

- ✅ Page load times
- ✅ Interaction response times
- ✅ Memory usage
- ✅ Network request optimization

## Output Reports

The QA agent generates comprehensive reports:

### Location

```
Aria-Agent/output/qa-report.md
```

### Report Contents

1. **Test Execution Summary**
   - Development server status
   - Chrome MCP connection status
   - Test scope and duration

2. **Test Cases Executed**
   - Detailed test case descriptions
   - Expected vs actual results
   - Screenshots and evidence

3. **Visual Evidence**
   - Before/after screenshots
   - Desktop and mobile views
   - Console logs

4. **Performance Metrics**
   - Page load times
   - Interaction response
   - Memory usage

5. **Issues Found**
   - Critical, medium, and minor issues
   - Regression testing results

6. **Final Verdict**
   - APPROVED FOR PRODUCTION
   - NEEDS FIXES
   - REQUIRES RE-TESTING

## Chrome MCP Commands

The QA agent can use these Chrome MCP commands:

```bash
# Navigation
mcp__chrome_navigate(url)

# Screenshots
mcp__chrome_screenshot(path)

# Interaction
mcp__chrome_click(selector)
mcp__chrome_type(selector, text)

# Information
mcp__chrome_logs()
mcp__chrome_evaluate(script)

# Responsive testing
mcp__chrome_viewport(width, height)
mcp__chrome_wait_for(selector, timeout)
```

## Integration with Aria

The QA Agent integrates seamlessly with other Aria agents:

1. **Fix Generator** → Creates code fixes
2. **QA Automation** → Tests the fixes
3. **Verification** → Validates technical standards
4. **Memory** → Tracks QA patterns and trends

### Workflow

```
User Request → Azure Agent → Code Analyst → Fix Generator → QA Agent → Final Report
```

## Test Scenarios

### Language Selector Testing (Ticket #114644)

```bash
./run.sh "Test the language selector country flag fix"
```

**Test Cases:**

- Country flags visible on desktop Chrome
- Country flags visible on mobile viewport
- Dropdown opens and closes correctly
- Language selection functionality works
- Flag emojis render properly (🇺🇸, 🇪🇸, 🇮🇳)

### Authentication Testing

```bash
./run.sh "Test user login flow with credentials erik@mailinator.com"
```

**Test Cases:**

- Login form validation
- Authentication with test credentials
- Session persistence after page refresh
- Logout functionality

### Mobile Compatibility Testing

```bash
./run.sh "Test responsive design for mobile devices"
```

**Test Cases:**

- Layout adapts to mobile viewport
- Touch interactions work
- Mobile-specific features function
- No horizontal scrolling issues

## Troubleshooting

### Chrome MCP Not Available

**Problem:** Chrome MCP connection fails

**Solution:** Agent automatically falls back to manual testing mode with detailed instructions

**Manual Steps:**

1. Start development server: `npm run dev`
2. Open browser to `http://localhost:5173`
3. Login with test credentials
4. Follow manual test cases in qa-report.md

### Development Server Not Running

**Problem:** Cannot connect to development server

**Solution:**

```bash
# Start server
npm run dev

# Check if running
curl http://localhost:5173

# Check for port conflicts
lsof -i :5173
```

### Test Credentials Don't Work

**Problem:** Authentication fails with test credentials

**Solution:**

- Verify test account exists in your system
- Update credentials in `.env` file
- Check authentication service status

### Screenshots Not Captured

**Problem:** No screenshots in QA report

**Solution:**

- Check Chrome MCP screenshot permissions
- Verify output directory is writable
- Ensure browser window is not minimized

## Best Practices

1. **Always Review QA Reports**: Check qa-report.md after each automated test
2. **Test Critical Paths**: Prioritize testing user journeys critical to business
3. **Keep Credentials Updated**: Maintain test account credentials
4. **Monitor Performance**: Track performance metrics over time
5. **Document Issues**: Report any issues found during testing
6. **Update Test Cases**: Add new test cases as features are added

## Future Enhancements

- Integration with additional browsers (Firefox, Safari)
- Visual regression testing with pixel comparison
- API testing and validation
- Load and stress testing
- Accessibility testing (WCAG compliance)
- Security testing penetration checks

## Support

For issues or questions about the QA Automation Agent:

1. Check `Aria-Agent/output/qa-report.md` for detailed test results
2. Review Chrome MCP setup documentation
3. Verify test credentials are correct
4. Ensure development server is running
5. Check browser console for errors

---

**QA Automation Agent** · Part of Aria Multi-Agent System · v1.0
