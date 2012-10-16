drop table if exists mlm_user;
create table mlm_user (
  id					bigint unsigned not null auto_increment,
	username			varchar(128),
	email				varchar(128) not null,
	password_hash		char(64) not null,
	activation_key	    varchar(128) not null,
	user_type			int(1) not null default '0',
	status				int(1) not null default '0',
	lastvisit			datetime,
	createdat			timestamp default current_timestamp ,
	updatedat			datetime,
	primary key (id),
	unique(email)
) 
engine = innodb
default character set = utf8;

drop table if exists mlm_employee_type;
create table mlm_employee_type (
	id	bigint unsigned not null auto_increment,
    emp_type varchar(500),
    description text,
	primary key (id)

) 
engine = innodb
default character set = utf8;



drop table if exists mlm_employe;
create table mlm_employe (
	id					bigint unsigned not null auto_increment,
	username			varchar(128),
	email				varchar(128) not null,
	password_hash		char(64) not null,
	employee_type_id	int(1) not null default '0',
	last_visit			datetime,
	createdat			timestamp default current_timestamp ,
	updatedat			datetime,
	primary key (id),
	unique(email),
    constraint employee_type 
        foreign key (employee_type_id) references mlm_employee_type (id) 
        on delete no action on update no action
) 
engine = innodb
default character set = utf8;


drop table if exists mlm_user_referral;
create table mlm_user_referral (
	id            bigint unsigned not null auto_increment,
	email         varchar(128) not null,
	-- 0 not joined, 1 joined
	status        int default 0,
	referredby    bigint unsigned not null,
	createdat     timestamp not null default current_timestamp,
	updatedat     datetime default null,
	primary key (id),
	constraint mlm_referral_user 
		foreign key (referredby) references mlm_user (id) 
		on delete no action on update no action
) engine=innodb 
default charset=utf8;


drop table if exists mlm_employee_referral;
create table mlm_employee_referral (
	id            bigint unsigned not null auto_increment,
	email         varchar(128) not null,
	-- 0 not joined, 1 joined
	status        int default 0,
	referredby    bigint unsigned not null,
	createdat     timestamp not null default current_timestamp,
	updatedat     datetime default null,
	primary key (id),
	constraint mlm_referral_employee 
		foreign key (referredby) references mlm_employe (id) 
		on delete no action on update no action
) engine=innodb 
default charset=utf8;

-- -----------------------------------------------------
-- table stage1
-- -----------------------------------------------------
drop table if exists stage1;
create table stage1 (
  id bigint unsigned not null auto_increment,
  user_id bigint unsigned not null,
  parent_id bigint unsigned,
  nleft bigint unsigned not null,
  nright bigint unsigned not null,
  createdat timestamp not null default current_timestamp,
  updatedat datetime default null,
  primary key (id),
  constraint stage1_user
        foreign key (user_id) references mlm_user (id) 
        on delete no action on update no action,
  constraint stage1_parent
        foreign key (parent_id) references mlm_user (id) 
        on delete no action on update no action
) engine=innodb 
default charset=utf8;


-- -----------------------------------------------------
-- table stage2
-- -----------------------------------------------------
drop table if exists stage2;
create table stage2 (
  id bigint unsigned not null auto_increment,
  user_id bigint unsigned not null,
  parent_id bigint unsigned,
  nleft bigint unsigned not null,
  nright bigint unsigned not null,
  createdat timestamp not null default current_timestamp,
  updatedat datetime default null,
  primary key (id),
  constraint stage2_user
        foreign key (user_id) references mlm_user (id) 
        on delete no action on update no action,
  constraint stage2_parent
        foreign key (parent_id) references mlm_user (id) 
        on delete no action on update no action
) engine=innodb 
default charset=utf8;


-- -----------------------------------------------------
-- table stage3
-- -----------------------------------------------------
drop table if exists stage3;
create table stage3 (
  id bigint unsigned not null auto_increment,
  user_id bigint unsigned not null,
  parent_id bigint unsigned,
  nleft bigint unsigned not null,
  nright bigint unsigned not null,
  createdat timestamp not null default current_timestamp,
  updatedat datetime default null,
  primary key (id),
  constraint stage3_user
        foreign key (user_id) references mlm_user (id) 
        on delete no action on update no action,
  constraint stage3_parent
        foreign key (parent_id) references mlm_user (id) 
        on delete no action on update no action
) engine=innodb 
default charset=utf8;