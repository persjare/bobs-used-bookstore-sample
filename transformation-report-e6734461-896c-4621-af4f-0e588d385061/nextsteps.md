# Next Steps

## Transformation Status

The transformation appears to be **successful** with no build errors reported across any of the projects in the solution:

- Bookstore.Data
- Bookstore.Domain.Tests
- Bookstore.Domain
- Bookstore.Web
- Bookstore.Cdk

## Recommended Validation Steps

### 1. Verify Build Configuration

```bash
# Clean and rebuild the entire solution
dotnet clean
dotnet build --configuration Release
```

Ensure both Debug and Release configurations build successfully.

### 2. Review Target Framework

Verify that all projects are targeting the appropriate .NET version:

```bash
# Check target frameworks for all projects
grep -r "<TargetFramework>" *.csproj
```

Ensure consistency across projects where appropriate (e.g., class libraries should target compatible frameworks).

### 3. Run Unit Tests

Execute all test projects to validate functionality:

```bash
# Run tests with detailed output
dotnet test --verbosity normal

# Generate code coverage report (optional)
dotnet test --collect:"XPlat Code Coverage"
```

Review test results for any failures or warnings that may indicate runtime compatibility issues.

### 4. Validate Dependencies

Check for deprecated or incompatible NuGet packages:

```bash
# List outdated packages
dotnet list package --outdated

# Check for vulnerable packages
dotnet list package --vulnerable
```

Update any packages that have cross-platform compatible versions available.

### 5. Test Database Connectivity (Bookstore.Data)

- Verify connection strings are platform-agnostic (avoid Windows-specific paths)
- Test database migrations and data access operations
- Confirm Entity Framework Core (or other ORM) operations work correctly

### 6. Validate Web Application (Bookstore.Web)

- Run the web application locally:
  ```bash
  cd Bookstore.Web
  dotnet run
  ```
- Test all endpoints and functionality
- Verify static files, views, and assets load correctly
- Check for any hardcoded Windows-specific paths (e.g., `C:\`, backslashes)

### 7. Review AWS CDK Project (Bookstore.Cdk)

- Ensure CDK constructs are compatible with the new .NET version
- Synthesize the CloudFormation template:
  ```bash
  cd Bookstore.Cdk
  cdk synth
  ```
- Validate that infrastructure definitions are correct

### 8. Check for Platform-Specific Code

Search for potential platform-specific issues:

- File path separators (use `Path.Combine()` instead of hardcoded slashes)
- Registry access (Windows-only)
- P/Invoke calls to Windows APIs
- Case-sensitive file system assumptions

### 9. Test on Target Platforms

Run the application on the intended target platforms:

- Linux (if targeting Linux deployment)
- macOS (if applicable)
- Windows (to ensure backward compatibility)

### 10. Review Configuration Files

- Validate `appsettings.json` and environment-specific configurations
- Ensure logging providers are cross-platform compatible
- Check that file paths in configuration use platform-agnostic formats

### 11. Performance Testing

- Conduct basic performance testing to identify any regressions
- Monitor memory usage and startup time
- Compare metrics with the legacy version if baseline data exists

### 12. Update Documentation

- Document the new target framework and runtime requirements
- Update build and deployment instructions
- Note any breaking changes or behavioral differences

## Deployment Preparation

Once validation is complete:

1. **Create a deployment package:**
   ```bash
   dotnet publish -c Release -o ./publish
   ```

2. **Test the published output** in an environment that mirrors production

3. **Update deployment scripts** to use `dotnet` commands instead of framework-specific tools

4. **Verify environment variables** and configuration sources are properly set for the target environment

## Final Verification Checklist

- [ ] Solution builds without errors in both Debug and Release modes
- [ ] All unit tests pass
- [ ] Integration tests (if any) pass
- [ ] Web application runs and responds correctly
- [ ] Database operations function properly
- [ ] No hardcoded platform-specific paths remain
- [ ] Dependencies are up-to-date and compatible
- [ ] Application tested on target deployment platform
- [ ] CDK infrastructure synthesizes correctly
- [ ] Documentation updated