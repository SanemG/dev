# Dev

A new Flutter project for the GitHub Actions Hackathon!.

This GitHub Action "Flutter Web CI" is designed to automate the developers work to: 
* Configure the flutter tool for web support
* Build an app with web support
* Archive the build and upload the Artifact
* Download the Artifact
* Display structure of build files
* Use GitHub Pages to host your Flutter Web app

## Usage

```yaml
name: Flutter Web CI

on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      my_secret: ${{secrets.GH_DEPLOY}}
    steps:
    - uses: actions/checkout@v1
    - uses: subosito/flutter-action@v1
      with:
        channel: beta
    - run: flutter config --enable-web
    - run: flutter pub get
#     - run: flutter test
    - run: flutter build web
    - name: Archive Production Artifact
      uses: actions/upload-artifact@master
      with:
        name: web-build
        path: build/web
    - name: Download Artifact
      uses: actions/download-artifact@master
      with:
        name: web-build 
    - name: Display structure of build files
      run: ls -R
      working-directory: ./web
    - name: Deploy to GH Pages
      run: |
        cd build/web
        git init
        # type configurations: your user.email and user.name followed lines 
        # git config --global user.email your_email 
        # git config --global user.name your_name 
        git config --global user.email ranubhardwaj89@gmail.com
        git config --global user.name himanshusharma89
        git status
        # change this remote url for examle your remote url is https://github.com/onatcipli/flutter_web.git then the following:
        git remote add origin https://${{secrets.GH_DEPLOY}}@github.com/himanshusharma89/dev.git
        git checkout -b gh-pages
        git add --all
        git commit -m "update"
        git push origin gh-pages -f

```
### Resources

A few resources to get you started with Flutter project:

- [Lab: Write your first Flutter app](https://flutter.dev/docs/get-started/codelab)
- [Cookbook: Useful Flutter samples](https://flutter.dev/docs/cookbook)
