# IAM_Troubleshooting

# Granting Permissions for AWSLabsUser to Attach Policies to Roles

In this guide, we'll go over various options to allow the `AWSLabsUser` in AWS to attach policies to roles, including both predefined and custom policies.

## Issue Overview
When trying to attach a policy to a role, the action `iam:AttachRolePolicy` fails because there is no existing identity-based policy allowing this action for `AWSLabsUser`. This guide provides different options for granting this permission.
![Flow](https://github.com/user-attachments/assets/297c16ee-7984-412f-894f-9f51ee0d97ec)

---

## Option 1: Attach the IAMFullAccess Policy

### Description
The `IAMFullAccess` managed policy grants full permissions over IAM resources, including the ability to manage users, roles, policies, and groups.

### Usage

1. Go to the IAM console.
2. Attach the `IAMFullAccess` policy to the `AWSLabsUser` user.

#### Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:*",
      "Resource": "*"
    }
  ]
}


# Option 2: Attach the PowerUserAccess Policy with Additional Custom Permissions

## Description
The `PowerUserAccess` policy provides broad administrative access except for IAM management. You can attach this policy along with a custom policy to allow only specific IAM actions, such as attaching policies to roles.

## Usage

1. Attach `PowerUserAccess` to the `AWSLabsUser`.
2. Create a custom policy allowing `iam:AttachRolePolicy` for specific roles.

### Custom Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:AttachRolePolicy",
      "Resource": "arn:aws:iam::<AccountID>:role/TargetRoleName"
    }
  ]
}


# Option 3: Allow Role Switching with Cross-Account Access

If you need `AWSLabsUser` to attach policies across accounts, you can use a combination of permissions and trust policies.

## Steps

1. **In the Source Account**: Attach a policy to `AWSLabsUser` allowing `sts:AssumeRole` on the target account role.
2. **In the Target Account**: Modify the role’s trust policy to allow `AWSLabsUser` to assume it.

### Source Account Policy Example

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "sts:AssumeRole",
      "Resource": "arn:aws:iam::<TargetAccountID>:role/TargetRoleName"
    }
  ]
}
