- To get the IAM user we use the following command:
```
aws --profile {profile-name} iam get-user
```

- To list the attached policies to a given user we use the following command:
```
aws --profile {profile-name} iam list-attached-user-policies --user-name {username}
```

