# Theoremus Setup Guide

This document describes how to maintain and publish this forked package for Theoremus organization.

## Package Information

- **Package Name**: `@theoremus/react-native-wallet-manager`
- **Registry**: NPM Public Registry
- **Original Package**: [react-native-wallet-manager](https://github.com/theoremus-urban-solutions/react-native-wallet-manager) by dev-family
- **Repository**: https://github.com/theoremus/react-native-wallet-manager

## Initial Setup

### 1. NPM Organization Setup

Before publishing, ensure you have access to the `@theoremus` NPM organization:

```bash
# Login to NPM
npm login

# Verify you have access to @theoremus organization
npm access ls-packages theoremus
```

If the organization doesn't exist yet, create it on npmjs.com:
- Go to https://www.npmjs.com/org/create
- Create organization named `theoremus`
- Add team members who should have publish access

### 2. Repository Setup

Update your git remote to point to your organization's repository:

```bash
# Check current remote
git remote -v

# Update remote URL if needed
git remote set-url origin https://github.com/theoremus/react-native-wallet-manager.git

# Or add as new remote
git remote add theoremus https://github.com/theoremus/react-native-wallet-manager.git
```

### 3. First Time Build

Install dependencies and build the library:

```bash
# Install dependencies
yarn install

# Build the library
yarn prepare

# Run linting and type checking
yarn lint
yarn typescript

# Run tests
yarn test
```

## Publishing

### Manual Publishing

1. **Update version** in `package.json`:
   ```bash
   npm version patch  # for bug fixes
   npm version minor  # for new features
   npm version major  # for breaking changes
   ```

2. **Build the package**:
   ```bash
   yarn prepare
   ```

3. **Publish to NPM**:
   ```bash
   npm publish --access public
   ```

### Using release-it (Recommended)

The package is configured with `release-it` for automated releases:

```bash
# Dry run to see what would happen
yarn release --dry-run

# Create a release (this will bump version, create git tag, and publish)
yarn release
```

This will:
- Bump the version according to conventional commits
- Update CHANGELOG.md
- Create a git tag
- Push to GitHub
- Publish to NPM
- Create a GitHub release

## Development Workflow

### Adding New Features

1. Create a feature branch:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. Make your changes and test thoroughly

3. Commit with conventional commit messages:
   ```bash
   git commit -m "feat: add new wallet feature"
   git commit -m "fix: resolve issue with pass loading"
   ```

4. Push and create a pull request:
   ```bash
   git push origin feature/your-feature-name
   ```

5. After merge, publish a new version from main branch

### Syncing with Upstream

To pull updates from the original repository:

```bash
# Add upstream remote (first time only)
git remote add upstream https://github.com/theoremus-urban-solutions/react-native-wallet-manager.git

# Fetch upstream changes
git fetch upstream

# Merge upstream changes
git merge upstream/main

# Resolve any conflicts, then push
git push origin main
```

## Testing Before Publishing

1. **Test in the example app**:
   ```bash
   cd example
   yarn install
   yarn ios     # or yarn android
   ```

2. **Test as a local package** in your app:
   ```bash
   # In your app's package.json
   {
     "dependencies": {
       "@theoremus/react-native-wallet-manager": "file:../react-native-wallet-manager"
     }
   }
   ```

3. **Test the published package**:
   ```bash
   # After publishing, install in a test app
   npm install @theoremus/react-native-wallet-manager@latest
   ```

## Maintenance

### Keeping Dependencies Updated

```bash
# Check for outdated dependencies
yarn outdated

# Update dependencies
yarn upgrade-interactive --latest
```

### Security Audits

```bash
# Check for security vulnerabilities
npm audit

# Fix automatically when possible
npm audit fix
```

## Common Issues

### Publishing Access Denied

If you get a 403 error when publishing:
1. Ensure you're logged in: `npm whoami`
2. Check organization access: `npm access ls-packages theoremus`
3. Contact organization admin to grant publish rights

### Build Failures

If `yarn prepare` fails:
1. Clean build artifacts: `rm -rf lib`
2. Reinstall dependencies: `rm -rf node_modules && yarn install`
3. Check TypeScript errors: `yarn typescript`

### iOS Pod Issues

If iOS builds fail:
1. Clean pods: `cd example/ios && pod deintegrate && pod install`
2. Clean derived data
3. Ensure Xcode is up to date

## Support

For questions or issues specific to the Theoremus fork, create an issue at:
https://github.com/theoremus/react-native-wallet-manager/issues

## License

This fork maintains the MIT license from the original project. See LICENSE file for details.
