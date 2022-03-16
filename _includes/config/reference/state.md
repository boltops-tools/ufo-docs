state.bucket | Can be set to use an existing bucket. When not set ufo creates a managed s3 bucket. This setting overrides the `managed` option. | nil
state.managed | Setting to `false` disable creation of managed bucket entirely. | true
state.reminder | Disable reminder message about committing the state file. When using storage provider file, a reminder a message to the user is shown to commit the changes to the local state file. | true
state.storage | Storage provider. IE: s3 or file | s3