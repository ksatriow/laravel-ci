# LaraCIGitAc (Laravel Continuous Integration Github Action) 

## Task

- Verify if we can install composer dependencies
- Verify if npm dependencies can be successfully installed
- Run the frontend assets build command to verify if we can successfully minify our css and js files
- Verify if we can run migrations on a database without any issues(We will use a temporary sqlite database for this)
- Run our unit tests and make sure the code changes in the pull request did not break existing functionality.


## Step

1. Setup PHP
   ``` yaml
    # YAML
       - name: Setup PHP
      uses: shivammathur/setup-php@v2
      with:
        php-version: 7.2 # Change to the php version your application is using
        extensions: mbstring, bcmath # Install any required php extensions
   ```
2. Create .env file
   ``` yaml
    # YAML
    - name: Copy .env.test to .env
      run: php -r "file_exists('.env') || copy('.env.test', '.env');"
   ```
3. Install Composer
   ``` yaml
    # YAML
    - name: Install composer dependencies
      run: composer install
   ```
4. Set Directory Permissions
   ``` yaml
    # YAML
    - name: Set required directory permissions
      run: chmod -R 777 storage bootstrap/cache
   ```
5. Generate encryption key
   ``` yaml
    # YAML
    - name: Generate encryption key
      run: php artisan key:generate
   ```
6. Create temporary database
   ``` yaml
    # YAML
    - name: Create temporary sqlite database
      run: |
        mkdir -p database
        touch database/database.sqlite
   ```
7. Database migrations
   ``` yaml
    # YAML
    - name: Run laravel database migrations
      env:
        DB_CONNECTION: sqlite
        DB_DATABASE: database/database.sqlite
      run: php artisan migrate --force
   ```
8. Install NPM
   ``` yaml
    # YAML
    - name: Install npm dependencies
      run: npm install
   ```
9. Minify CSS and JS Files
   ``` yaml
    # YAML
    - name: Minify CSS and JS files
      run: npm run prod
   ```
10. Run Unit Tests
   ``` yaml
    # YAML
    - name: Run Unit Tests via PHPUnit
      env:
        DB_CONNECTION: sqlite
        DB_DATABASE: database/database.sqlite
      run: ./vendor/bin/phpunit
   ```
