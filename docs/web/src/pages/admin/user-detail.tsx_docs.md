# Documentation: web/src/pages/admin/user-detail.tsx

## File Metadata

- **Path**: `web/src/pages/admin/user-detail.tsx`
- **Size**: 13290 bytes
- **Type**: .tsx
- **Readable**: Yes

## Purpose

This file is part of the RAGFlow repository at location `web/src/pages/admin/user-detail.tsx`.

## Original Source Code

```tsx
import { useMemo } from 'react';
import { useTranslation } from 'react-i18next';
import { useNavigate, useParams } from 'umi';

import { LucideArrowLeft, LucideDot } from 'lucide-react';

import { useQuery } from '@tanstack/react-query';
import {
  createColumnHelper,
  flexRender,
  getCoreRowModel,
  getSortedRowModel,
  useReactTable,
} from '@tanstack/react-table';

import { Routes } from '@/routes';

import { RAGFlowAvatar } from '@/components/ragflow-avatar';
import Spotlight from '@/components/spotlight';
import { Badge } from '@/components/ui/badge';
import { Button } from '@/components/ui/button';
import { Card, CardContent, CardHeader } from '@/components/ui/card';
import { RAGFlowPagination } from '@/components/ui/ragflow-pagination';
import { ScrollArea } from '@/components/ui/scroll-area';
import {
  Table,
  TableBody,
  TableCell,
  // TableHead,
  // TableHeader,
  TableRow,
} from '@/components/ui/table';
import { Tabs, TabsContent, TabsList, TabsTrigger } from '@/components/ui/tabs';

import {
  getUserDetails,
  listUserAgents,
  listUserDatasets,
} from '@/services/admin-service';

import { TableEmpty } from '@/components/table-skeleton';
import EnterpriseFeature from './components/enterprise-feature';
import {
  // getSortIcon,
  parseBooleanish,
} from './utils';

const ASSET_NAMES = ['dataset', 'flow'];

const datasetColumnHelper =
  createColumnHelper<AdminService.ListUserDatasetItem>();
const agentColumnHelper = createColumnHelper<AdminService.ListUserAgentItem>();

function UserDatasetTable(props: {
  data?: AdminService.ListUserDatasetItem[];
}) {
  const { t } = useTranslation();

  const columnDefs = useMemo(
    () => [
      datasetColumnHelper.accessor('name', {
        header: t('admin.name'),
        cell: ({ row, cell }) => (
          <div className="flex items-center gap-2">
            <RAGFlowAvatar
              avatar={row.original.avatar}
              name={cell.getValue()}
            />
            <span>{cell.getValue()}</span>
          </div>
        ),
      }),
      // #region
      /*
      datasetColumnHelper.accessor('name', {
        header: t('admin.name'),
        enableSorting: false,
      }),
      datasetColumnHelper.accessor('status', {
        header: t('admin.status'),
        cell: ({ cell }) => {
          return (
            <Badge
              variant={parseBooleanish(cell.getValue()) ? 'success' : 'destructive'}
              className="pl-[.35em]"
            >
              <LucideDot className="size-[1em] stroke-[8] mr-1" />
              {t(
                parseBooleanish(cell.getValue())
                  ? 'admin.active'
                  : 'admin.inactive',
              )}"
            </Badge>
          );
        },
        enableSorting: false,
      }),
      datasetColumnHelper.accessor('chunk_num', {
        header: t('admin.chunkNum'),
      }),
      datasetColumnHelper.accessor('doc_num', {
        header: t('admin.docNum'),
      }),
      datasetColumnHelper.accessor('token_num', {
        header: t('admin.tokenNum'),
      }),
      datasetColumnHelper.accessor('language', {
        header: t('admin.language'),
        enableSorting: false,
      }),
      datasetColumnHelper.accessor('create_date', {
        header: t('admin.createDate'),
      }),
      datasetColumnHelper.accessor('update_date', {
        header: t('admin.updateDate'),
      }),
      datasetColumnHelper.accessor('permission', {
        header: t('admin.permission'),
        enableSorting: false,
      }),
      */
      // #endregion
    ],
    [t],
  );

  const table = useReactTable({
    data: props.data ?? [],
    columns: columnDefs,

    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),

    enableSorting: false,
  });

  return (
    <section className="space-y-4">
      <Table>
        {/* <TableHeader>
          {table.getHeaderGroups().map((headerGroup) => (
            <TableRow key={headerGroup.id}>
              {headerGroup.headers.map((header) => (
                <TableHead key={header.id}>
                  {header.isPlaceholder ? null : header.column.getCanSort() ? (
                    <Button
                      variant="ghost"
                      onClick={header.column.getToggleSortingHandler()}
                    >
                      {flexRender(
                        header.column.columnDef.header,
                        header.getContext(),
                      )}
                      {getSortIcon(header.column.getIsSorted())}
                    </Button>
                  ) : (
                    flexRender(
                      header.column.columnDef.header,
                      header.getContext(),
                    )
                  )}
                </TableHead>
              ))}
            </TableRow>
          ))}
        </TableHeader> */}

        <TableBody>
          {table.getRowModel().rows?.length ? (
            table.getRowModel().rows.map((row) => (
              <TableRow key={row.id}>
                {row.getVisibleCells().map((cell) => (
                  <TableCell key={cell.id}>
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
                  </TableCell>
                ))}
              </TableRow>
            ))
          ) : (
            <TableEmpty columnsLength={columnDefs.length} />
          )}
        </TableBody>
      </Table>

      <RAGFlowPagination
        total={props.data?.length}
        current={table.getState().pagination.pageIndex + 1}
        pageSize={table.getState().pagination.pageSize}
        onChange={(page, pageSize) => {
          table.setPagination({
            pageIndex: page - 1,
            pageSize,
          });
        }}
      />
    </section>
  );
}

function UserAgentTable(props: { data?: AdminService.ListUserAgentItem[] }) {
  const { t } = useTranslation();

  const columnDefs = useMemo(
    () => [
      agentColumnHelper.accessor('title', {
        header: t('admin.agentTitle'),
        cell: ({ row, cell }) => (
          <div className="flex items-center gap-2">
            <RAGFlowAvatar
              avatar={row.original.avatar}
              name={cell.getValue()}
            />
            <span>{cell.getValue()}</span>
          </div>
        ),
      }),
      // #region
      /*
      agentColumnHelper.accessor('title', {
        header: t('admin.agentTitle'),
      }),
      agentColumnHelper.accessor('permission', {
        header: t('admin.permission'),
      }),
      agentColumnHelper.accessor('canvas_category', {
        header: t('admin.canvasCategory'),
      }),
      */
      // #endregion
    ],
    [t],
  );

  const table = useReactTable({
    data: props.data ?? [],
    columns: columnDefs,

    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),

    enableSorting: false,
  });

  return (
    <section className="space-y-4">
      <Table>
        {/* <TableHeader>
          {table.getHeaderGroups().map((headerGroup) => (
            <TableRow key={headerGroup.id}>
              {headerGroup.headers.map((header) => (
                <TableHead key={header.id}>
                  {header.isPlaceholder ? null : (
                    <>
                      {flexRender(
                        header.column.columnDef.header,
                        header.getContext(),
                      )}
                    </>
                  )}
                </TableHead>
              ))}
            </TableRow>
          ))}
        </TableHeader> */}

        <TableBody>
          {table.getRowModel().rows?.length ? (
            table.getRowModel().rows.map((row) => (
              <TableRow key={row.id}>
                {row.getVisibleCells().map((cell) => (
                  <TableCell key={cell.id}>
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
                  </TableCell>
                ))}
              </TableRow>
            ))
          ) : (
            <TableEmpty columnsLength={columnDefs.length} />
          )}
        </TableBody>
      </Table>

      <RAGFlowPagination
        total={props.data?.length}
        current={table.getState().pagination.pageIndex + 1}
        pageSize={table.getState().pagination.pageSize}
        onChange={(page, pageSize) => {
          table.setPagination({
            pageIndex: page - 1,
            pageSize,
          });
        }}
      />
    </section>
  );
}

function AdminUserDetail() {
  const navigate = useNavigate();
  const { t } = useTranslation();
  const { id } = useParams();

  const { data: { detail, datasets, agents } = {} } = useQuery({
    queryKey: ['admin/userDetail', id],
    queryFn: async () => {
      const [userDetails, userDatasets, userAgents] = await Promise.all([
        getUserDetails(id!),
        listUserDatasets(id!),
        listUserAgents(id!),
      ]);

      return {
        detail: userDetails.data.data[0],
        datasets: userDatasets.data.data,
        agents: userAgents.data.data,
      };
    },
    enabled: !!id,
    retry: false,
  });

  return (
    <section className="px-10 py-5 size-full flex flex-col">
      <nav className="mb-5">
        <Button
          variant="outline"
          className="h-10 px-3 dark:bg-bg-input dark:border-border-button"
          onClick={() => navigate(`${Routes.AdminUserManagement}`)}
        >
          <LucideArrowLeft />
          <span>{t('admin.back')}</span>
        </Button>
      </nav>

      <Card className="!shadow-none relative h-0 basis-0 grow flex flex-col bg-transparent border-0.5 border-border-button overflow-hidden">
        <Spotlight />

        <CardHeader className="pb-10 border-b-0.5 dark:border-border-button space-y-8">
          <section className="flex items-center gap-4 text-base">
            <RAGFlowAvatar
              avatar={detail?.avatar}
              name={detail?.email}
              isPerson
            />

            <span>{detail?.email}</span>

            <Badge
              variant={
                parseBooleanish(detail?.is_active) ? 'success' : 'destructive'
              }
              className="pl-[.5em]"
            >
              <LucideDot className="size-[1em] stroke-[8] mr-1" />
              {t(
                parseBooleanish(detail?.is_active)
                  ? 'admin.active'
                  : 'admin.inactive',
              )}
            </Badge>

            <EnterpriseFeature>
              {() =>
                detail?.role && (
                  <Badge variant="secondary">{detail?.role}</Badge>
                )
              }
            </EnterpriseFeature>
          </section>

          <section className="flex items-start px-14 space-x-14">
            <div>
              <div className="text-sm text-text-secondary mb-2">
                {t('admin.lastLoginTime')}
              </div>
              <div>{detail?.last_login_time}</div>
            </div>

            <div>
              <div className="text-sm text-text-secondary mb-2">
                {t('admin.createTime')}
              </div>
              <div>{detail?.create_date}</div>
            </div>

            <div>
              <div className="text-sm text-text-secondary mb-2">
                {t('admin.lastUpdateTime')}
              </div>
              <div>{detail?.update_date}</div>
            </div>

            <div>
              <div className="text-sm text-text-secondary mb-2">
                {t('admin.language')}
              </div>
              <div>{detail?.language}</div>
            </div>

            <div>
              <div className="text-sm text-text-secondary mb-2">
                {t('admin.isAnonymous')}
              </div>
              <div>{t(detail?.is_anonymous ? 'admin.yes' : 'admin.no')}</div>
            </div>

            <div>
              <div className="text-sm text-text-secondary mb-2">
                {t('admin.isSuperuser')}
              </div>
              <div>{t(detail?.is_superuser ? 'admin.yes' : 'admin.no')}</div>
            </div>
          </section>
        </CardHeader>

        <CardContent className="h-0 basis-0 grow pt-6">
          <Tabs className="h-full flex flex-col" defaultValue="dataset">
            <TabsList className="p-0 mb-2 gap-4 bg-transparent justify-start">
              {ASSET_NAMES.map((name) => (
                <TabsTrigger
                  key={name}
                  className="text-text-secondary border-0.5 border-border-button data-[state=active]:bg-bg-card"
                  value={name}
                >
                  {t(`header.${name}`)}
                </TabsTrigger>
              ))}
            </TabsList>

            <TabsContent value="dataset" className="h-0 basis-0 grow">
              <ScrollArea className="h-full">
                <UserDatasetTable data={datasets} />
              </ScrollArea>
            </TabsContent>

            <TabsContent value="flow" className="h-0 basis-0 grow">
              <ScrollArea className="h-full">
                <UserAgentTable data={agents} />
              </ScrollArea>
            </TabsContent>
          </Tabs>
        </CardContent>
      </Card>
    </section>
  );
}

export default AdminUserDetail;

```

## Detailed Analysis

### File Role in Repository

The file `web/src/pages/admin/user-detail.tsx` is located in the `web/src/pages/admin` directory.

This file is part of the **Frontend/Web** layer of RAGFlow.

### Architecture Context

Files in this location typically handle concerns related to admin.

### Design Patterns

[Analysis of design patterns would go here based on code structure]

### Performance Considerations

[Performance analysis would consider file size, complexity, algorithmic efficiency]

### Security Considerations

- Watch for XSS vulnerabilities
- Ensure proper input sanitization
- Validate all API calls

### Testing Approach

To test this file:
1. Review the corresponding test files in the test/ directory
2. Ensure all public APIs have test coverage
3. Test edge cases and error conditions
4. Verify integration with related components

### Related Files

- [login.tsx](login.tsx_docs.md)
- [monitoring.tsx](monitoring.tsx_docs.md)
- [roles.tsx](roles.tsx_docs.md)
- [service-detail.tsx](service-detail.tsx_docs.md)
- [service-status.tsx](service-status.tsx_docs.md)
- [task-executor-detail.tsx](task-executor-detail.tsx_docs.md)
- [users.tsx](users.tsx_docs.md)
- [utils.tsx](utils.tsx_docs.md)
- [whitelist.tsx](whitelist.tsx_docs.md)


## Cross-References

- [Folder Documentation](./doc.md)
- [Folder Index](./index.md)
- [Global Index](../../index.md)

---

*Generated by RAGFlow Comprehensive Documentation Generator*
