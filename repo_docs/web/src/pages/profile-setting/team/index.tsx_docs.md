# File Documentation: web/src/pages/profile-setting/team/index.tsx

## File Metadata

- **Path**: `web/src/pages/profile-setting/team/index.tsx`
- **Extension**: `.tsx`
- **Lines**: 108
- **Characters**: 3,649
- **Size**: 3,649 bytes
- **Purpose**: JavaScript/TypeScript - Frontend or backend JavaScript code

## Original Source

```tsx
import { Button } from '@/components/ui/button';
import { Card } from '@/components/ui/card';
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from '@/components/ui/dropdown-menu';
import { Table, TableBody, TableCell, TableRow } from '@/components/ui/table';
import { ChevronDown, MoreVertical, Plus, UserPlus } from 'lucide-react';

interface TeamMember {
  email: string;
  name: string;
  role: string;
}

const TeamManagement = () => {
  const teamMembers: TeamMember[] = [
    { email: 'yifanwu92@gmail.com', name: 'Yifan Wu', role: 'Admin' },
    { email: 'yifanwu92@gmail.com', name: 'Yifan Wu', role: 'Admin' },
  ];

  const stats = {
    project: 1,
    token: '1,000',
    storage: '1GB',
  };

  return (
    <div className="p-8 ">
      <div className=" mx-auto">
        <div className="flex justify-between items-center mb-8">
          <h1 className="text-4xl font-bold">Team management</h1>
          <Button size={'sm'}>
            <Plus className="mr-2 h-4 w-4" />
            Create team
          </Button>
        </div>

        <div className="mb-8">
          <div className="flex justify-between items-center mb-4">
            <h2 className="text-2xl font-semibold">Yifan's team</h2>
            <Button variant="secondary" size="icon">
              <ChevronDown className="h-4 w-4" />
            </Button>
          </div>

          <Card className="border-0 p-6 mb-6">
            <div className="grid grid-cols-3 gap-8">
              <div>
                <p className="text-sm text-gray-400 mb-2">Project</p>
                <p className="text-2xl font-semibold">{stats.project}</p>
              </div>
              <div>
                <p className="text-sm text-gray-400 mb-2">Token</p>
                <p className="text-2xl font-semibold">{stats.token}</p>
              </div>
              <div>
                <p className="text-sm text-gray-400 mb-2">Storage</p>
                <p className="text-2xl font-semibold">{stats.storage}</p>
              </div>
            </div>
          </Card>

          <Card className="border-0 p-6">
            <Table>
              <TableBody>
                {teamMembers.map((member, idx) => (
                  <TableRow key={idx}>
                    <TableCell>{member.email}</TableCell>
                    <TableCell>{member.name}</TableCell>
                    <TableCell className="flex items-center justify-end">
                      <span className="text-colors-text-core-standard">
                        {member.role}
                      </span>
                      <DropdownMenu>
                        <DropdownMenuTrigger asChild>
                          <Button variant="ghost" size="icon">
                            <MoreVertical className="h-4 w-4" />
                          </Button>
                        </DropdownMenuTrigger>
                        <DropdownMenuContent align="end">
                          <DropdownMenuItem>Edit</DropdownMenuItem>
                          <DropdownMenuItem className="text-red-600">
                            Remove
                          </DropdownMenuItem>
                        </DropdownMenuContent>
                      </DropdownMenu>
                    </TableCell>
                  </TableRow>
                ))}
              </TableBody>
            </Table>

            <Button variant="outline" className="mt-6 ">
              <UserPlus className="mr-2 h-4 w-4" />
              Invite member
            </Button>
          </Card>
        </div>
      </div>
    </div>
  );
};

export default TeamManagement;

```

## High-Level Overview

This file is part of the RAGFlow repository located at `web/src/pages/profile-setting/team/index.tsx`.

Based on the file structure and naming, it appears to be a javascript/typescript - frontend or backend javascript code.

The file contains approximately 108 lines of code and defines various components
that contribute to the overall functionality of the RAGFlow system.

## Detailed Walkthrough


### Functions (1)

- `TeamManagement()`: Function definition

### Imports (5)

- `import { Button } from '@/components/ui/button';`
- `import { Card } from '@/components/ui/card';`
- `import {`
- `import { Table, TableBody, TableCell, TableRow } from '@/components/ui/table';`
- `import { ChevronDown, MoreVertical, Plus, UserPlus } from 'lucide-react';`

## Code Structure Analysis

- Total lines: 108
- Blank lines: 10 (9.3%)
- Comment lines: ~0 (0.0%)
- Code lines: ~98


## Dependencies and Imports

- `@/components/ui/button`
- `@/components/ui/card`
- `@/components/ui/table`
- `lucide-react`

## Design & Architecture

This file is located in the `web` directory, specifically within `web/src/pages/profile-setting/team`.

This appears to be a UI component or frontend module.

## Performance & Complexity

- No specific performance concerns identified through static analysis

## Security & Safety Considerations

- No immediate security concerns identified through static analysis

## Testing & Usage Notes

To work with this file:
1. Understand its dependencies (see Dependencies section)
2. Review the code structure and main components
3. Check for existing tests in the test directories
4. Consider edge cases and error handling

## Related Files

- Other files in `web/src/pages/profile-setting/team/` directory
- Potential test file: `test_index.tsx`

## Keywords

@/components/ui/button, @/components/ui/card, @/components/ui/table, Admin, Button, Card, ChevronDown, Create, DropdownMenu, DropdownMenuContent, DropdownMenuItem, DropdownMenuTrigger, Edit, Invite, MoreVertical, Plus, Project, Remove, Storage, Table, TableBody, TableCell, TableRow, Team, TeamManagement, TeamMember, Token, TypeScript, UserPlus, Yifan, gmail, lucide-react, stats, teamMembers

---
*Generated by RAGFlow Repository Documentation Generator*
