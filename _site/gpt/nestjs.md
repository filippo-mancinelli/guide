## Insert to supabase with DTO:
Yes, in NestJS you can directly pass the DTO object instance to insert data using Supabase. Your current implementation is already close to this approach. Here's how you can refine it:

1. First, ensure that your `CreateTgUserDto` class properties match exactly with your Supabase table column names. If they don't, you'll need to use a mapping or transformation step.
2. You can then pass the `userToCreate` DTO directly to the insert method. Here's the modified `create` method:

```typescript
Copyasync create(userToCreate: CreateTgUserDto): Promise<CreateTgUserDto | null> {
  try {
    const { data, error } = await this.SupabaseService.getClient()
      .from('Users')
      .insert(userToCreate)
      .select()
      .single();

    if (error) {
      throw error;
    }

    return data ? plainToInstance(CreateTgUserDto, data) : null;
  } catch (error) {
    console.error('Error creating user:', error);
    return null;
  }
}
```

This approach has several benefits:

1. It directly uses the DTO object, reducing the need for manual property mapping.
2. By adding `.select().single()` after the insert, we get the inserted row back, which we can then convert back to our DTO type.
3. We use `plainToInstance` to convert the returned data back into our DTO type, ensuring type safety.

Remember that for this to work seamlessly:

* The properties in your `CreateTgUserDto` should match the column names in your Supabase 'Users' table.
* If there's a mismatch between DTO property names and table column names, you might need to use class-transformer decorators in your DTO to map the properties correctly.

Also, note that I changed the return type to `CreateTgUserDto | null` to match what the function is actually returning.

Would you like me to explain any part of this code or provide more information about using DTOs with Supabase in NestJS?

### Response:
Thank you for providing the table structure. I'll adjust the NestJS code to match the Supabase table structure you've shared. Here's the modified code:

First, let's update the `CreateTgUserDto`:

```typescript
Copyexport class CreateTgUserDto {
  username?: string;
  role: string = 'CUSTOMER';
  tenant_id: number;
  platform?: string;
  telegram_user_id?: number;
}
```

Here are the key changes and explanations:

1. In `CreateTgUserDto`:
   * Changed `tenantId` to `tenant_id` to match the table column name.
   * Made `username`, `platform`, and `telegram_user_id` optional with `?` as they can be null in the table.
   * Set a default value for `role` as 'CUSTOMER'.
2. In `UsersService`:
   * The `create` method now returns `Promise<User | null>` instead of `Promise<CreateTgUserDto | null>`, as we're returning the full user entity after creation.
   * In `checkUserExistence`:
     * Changed parameter names to match table column names.
     * Updated the method to use Supabase's `single()` method and handle potential errors.
     * Returns `null` if no user is found or if there's an error.
3. We're using `plainToInstance(User, data)` to convert the returned data into a `User` entity. Make sure you have a `User` entity class that matches your table structure.
4. The `findAll`, `findOne`, and `remove` methods are left as placeholders. You should implement these based on your specific requirements.

Remember to create a `User` entity class that matches your table structure:

```typescript
Copyexport class User {
  id: number;
  created_at: Date;
  uuid_user: string;
  role: string;
  tenant_id: number;
  username?: string;
  platform?: string;
  telegram_user_id?: number;
}
```

This structure now aligns with your Supabase table and should work correctly with the DTO and service methods.
