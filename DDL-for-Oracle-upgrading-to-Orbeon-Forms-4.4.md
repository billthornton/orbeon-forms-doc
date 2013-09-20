```sql
alter table orbeon_form_definition        rename column last_modified  to last_modified_time; 
alter table orbeon_form_definition        rename column username       to last_modified_by;
alter table orbeon_form_definition_attach rename column last_modified  to last_modified_time; 
alter table orbeon_form_definition_attach rename column username       to last_modified_by;
alter table orbeon_form_data              rename column last_modified  to last_modified_time; 
alter table orbeon_form_data              rename column username       to last_modified_by;
alter table orbeon_form_data_attach       rename column last_modified  to last_modified_time; 
alter table orbeon_form_data_attach       rename column username       to last_modified_by;
alter table orbeon_form_data              add           username       varchar2(255);
alter table orbeon_form_data              add           groupname      varchar2(255);
alter table orbeon_form_data_attach       add           username       varchar2(255);
alter table orbeon_form_data_attach       add           groupname      varchar2(255);
alter table orbeon_form_data              add           draft          character(1);
update      orbeon_form_data                        set draft = 'N';
alter table orbeon_form_data              modify        draft          character(1) not null;
alter table orbeon_form_data_attach       add           draft          character(1);
update      orbeon_form_data_attach                 set draft = 'N';
alter table orbeon_form_data_attach       modify        draft          character(1) not null;
```