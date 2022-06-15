# GitHub Actions Demo for Schema Registry Maven Plugin

## A CI/CD pipeline for managing schemas on the Confluent Schema Registry using GitHub Actions workflow

For this project we are going to use GitHub workflows to run actions triggered by events in our schemas lifecycle, in particular:
## Workflows triggered

1. Validate schema, check local schema compatibility, set compatibility of subject and schema
   compatibility with subject when a Pull Request is created to merge a new schema to
   master.            
   ```Validate -> Test-Local -> Set-Compatibility -> Test-Compatibility```
2. Register schema when a Pull Request is approved and merged to master.   
   ``` register ```

## Steps to manage schemas from development to deployment on the Confluent Schema Registry:
1. Create a new branch for your work: `git checkout -b [Branch Name]`.
2. Make changes by updating an existing schema or creating a new schema present in `src/main/resources`. In our example, changes to `flight.proto` or `order.avsc`
3. Add the changes to your branch: ``git add .``.
4. Commit the changes: ``git commit -m "New field on schemas xpto..."``.
5. Push the new branch to your remote: ``git push origin [Branch Name]``.
6. Open your remote branch and click "Compare & pull request" to create a Pull Request.
7. At this point the workflow in `.github/workflows/pull-request.yaml` will run the action to check schema compatibility and only if succeed will it create a new Pull Request:

```yml
run: mvn validate
```

```bash
# Individual steps look like
mvn schema-registry:validate@validate 
mvn schema-registry:test-local-compatibility@test-local 
mvn schema-registry:set-compatibility@set-compatibility 
mvn schema-registry:test-compatibility@test-compatibility 
````
9. If compatibility checking passes a new Pull Request is created for approval:
10. Final step is to approve the Pull Request and merge the changes to master:
11. At this point the workflow in `.github/workflows/push.yaml` will run the action to register the new schema on the Confluent Schema Registry:
```yml
run: mvn schema-registry:register@register
```
12. All done, your new schemas is now live on the Confluent Schema Registry!

GitHub actions workflow reference: https://docs.github.com/en/actions/learn-github-actions/understanding-github-actions