## Contribution Guidelines
Sorry, we are still new on this, sorting many things out right now. 

## Submit Code Changes
### GitFlow
This project used GitFlow [methodology](https://datasift.github.io/gitflow/IntroducingGitFlow.html). **All developers final commit is to develop branch**. Master branch is lock to team lead only. From there, team lead deploy to production environment.

## Create a release
1. Update `/public_html/.version` to increase the version number
2. Clearly log changes of this version at `/changelog.md in markdown format
3. When commit, build number in `/public_html/.build` should auto increase by git client
4. Always release from GITHUB page by creating a tag
5. Action `/.github/workflows/release.yml` automatically run upon release