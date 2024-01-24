name: Add to Project

on:
  issues:
    types:
      - opened

jobs:
  add_to_project:
    runs-on: ubuntu-latest
    steps:
    - name: Add to Project
      uses: actions/github-script@0.11.0
      with:
        script: |
          const payload = JSON.stringify({
            project_card: {
              content_id: context.payload.issue.id,
              content_type: 'Issue'
            }
          });
          const response = await octokit.request('POST /projects/columns/{column_id}/cards', {
            column_id: 'COLUMN_ID', // Replace with your column ID
            mediaType: {
              previews: [
                'inertia'
              ]
            },
            request: {
              data: payload,
              headers: {
                accept: 'application/vnd.github.inertia-preview+json'
              }
            }
          });
          console.log(response.data);
        token: ${{ secrets.GITHUB_TOKEN }}
