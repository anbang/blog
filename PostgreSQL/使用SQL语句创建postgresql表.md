语句如下


### 账户表创建


```postgresql
-- Table: public.accounts
 
-- DROP TABLE public.accounts;
 
CREATE TABLE public.accounts
(
    account text COLLATE pg_catalog."default" NOT NULL,
    type smallint,
    balance numeric,
    CONSTRAINT account_pkey PRIMARY KEY (account)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
COMMENT ON COLUMN public.accounts.account
    IS '账户地址';
 
COMMENT ON COLUMN public.accounts.type
    IS '账户类型（1普通账户、2合约账户）';
 
COMMENT ON COLUMN public.accounts.balance
    IS '余额，单位是Wei';
```

交易表创建

```postgresql
-- Table: public.transaction
 
-- DROP TABLE public.transaction;
 
CREATE TABLE public.transaction
(
    pkid bigserial,
    hash text COLLATE pg_catalog."default" NOT NULL,
    type numeric,
    "from" text COLLATE pg_catalog."default",
    "to" text COLLATE pg_catalog."default",
    amount numeric,
    previous text COLLATE pg_catalog."default",
    witness_list_block text COLLATE pg_catalog."default",
    last_summary text COLLATE pg_catalog."default",
    last_summary_block text COLLATE pg_catalog."default",
    data text COLLATE pg_catalog."default",
    exec_timestamp bigint,
    signature text COLLATE pg_catalog."default",
    is_free boolean,
    level bigint,
    witnessed_level bigint,
    best_parent text COLLATE pg_catalog."default",
    is_stable boolean,
    status numeric,
    is_on_mc boolean,
    is_witness_account boolean,
    mci bigint,
    latest_included_mci bigint,
    mc_timestamp bigint,
    is_witness boolean,
    stable_timestamp bigint,
    CONSTRAINT pkid_pkey PRIMARY KEY (pkid)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
COMMENT ON COLUMN public.transaction.hash
    IS '交易号';
 
COMMENT ON COLUMN public.transaction."from"
    IS '发款方';
 
COMMENT ON COLUMN public.transaction."to"
    IS '收款方';
 
COMMENT ON COLUMN public.transaction.amount
    IS '余额，单位是Wei';
 
COMMENT ON COLUMN public.transaction.previous
    IS '上一个交易号';
 
COMMENT ON COLUMN public.transaction.witness_list_block
    IS '见证人列表Block';
 
COMMENT ON COLUMN public.transaction.last_summary
    IS 'last_summary';
 
COMMENT ON COLUMN public.transaction.last_summary_block
    IS 'last_summary_block';
 
COMMENT ON COLUMN public.transaction.data
    IS '数据';
 
COMMENT ON COLUMN public.transaction.exec_timestamp
    IS 'exec_timestamp';
 
COMMENT ON COLUMN public.transaction.signature
    IS '签名';
 
COMMENT ON COLUMN public.transaction.is_free
    IS 'is_free';
 
COMMENT ON COLUMN public.transaction.level
    IS 'level';
 
COMMENT ON COLUMN public.transaction.witnessed_level
    IS 'witnessed_level';
 
COMMENT ON COLUMN public.transaction.best_parent
    IS 'best_parent';
 
COMMENT ON COLUMN public.transaction.is_stable
    IS 'is_stable';

 COMMENT ON COLUMN public.transaction.status
    IS 'status';

COMMENT ON COLUMN public.transaction.is_on_mc
    IS 'is_on_mc';

COMMENT ON COLUMN public.transaction.is_witness_account
    IS 'is_witness_account';
 
COMMENT ON COLUMN public.transaction.mci
    IS 'mci';
 
COMMENT ON COLUMN public.transaction.latest_included_mci
    IS 'latest_included_mci';
 
COMMENT ON COLUMN public.transaction.mc_timestamp
    IS 'mc_timestamp';
 
-- Index: hash_index
 
-- DROP INDEX public.hash_index;
 
CREATE UNIQUE INDEX hash_index
    ON public.transaction USING btree
    (hash COLLATE pg_catalog."default")
    TABLESPACE pg_default;
```

parent表创建

```postgresql
-- Table: public.parents
 
-- DROP TABLE public.parents;
 
CREATE TABLE public.parents
(
    parents_id bigserial,
    item text COLLATE pg_catalog."default" NOT NULL,
    parent text COLLATE pg_catalog."default",
    is_witness boolean,
    prototype text,
    CONSTRAINT parents_id_pkey PRIMARY KEY (parents_id),
    CONSTRAINT parents_item_parent_key UNIQUE (item, parent)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
COMMENT ON COLUMN public.parents.item
    IS '元素';
 
COMMENT ON COLUMN public.parents.parent
    IS 'parent';
```

witness表的创建

```postgresql
-- Table: public.witness
 
-- DROP TABLE public.witness;
 
CREATE TABLE public.witness
(
    witness_id bigserial,
    item text COLLATE pg_catalog."default" NOT NULL,
    account text COLLATE pg_catalog."default",
    CONSTRAINT witness_id_pkey PRIMARY KEY (witness_id),
    CONSTRAINT witness_item_account_key UNIQUE (item, account)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
COMMENT ON COLUMN public.witness.item
IS '元素';
 
COMMENT ON COLUMN public.witness.account
IS 'account';
```

MCI表的数据

```
-- Table: public.mci
 
-- DROP TABLE public.mci;
 
CREATE TABLE public.mci
(
    last_stable_mci numeric,
    last_mci numeric,
    CONSTRAINT last_stable_mci_key PRIMARY KEY (last_stable_mci)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
COMMENT ON COLUMN public.mci.last_stable_mci
    IS 'last_stable_mci';

COMMENT ON COLUMN public.mci.last_mci
    IS 'last_mci';
```

timestap表的创建

```
-- Table: public.timestamp
 
-- DROP TABLE public.timestamp;
 
CREATE TABLE public.timestamp
(
    timestamp numeric,
    type bigint,
    count numeric,
    CONSTRAINT timestamp_key PRIMARY KEY (timestamp)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
COMMENT ON COLUMN public.timestamp.timestamp
    IS 'timestamp';
 
COMMENT ON COLUMN public.timestamp.type
    IS 'type';

COMMENT ON COLUMN public.timestamp.count
    IS 'count';
```

## global

```
-- Table: public.global

-- DROP TABLE public.global;

CREATE TABLE public.global
(
    global_id bigserial,
    key text ,
    value numeric,
    CONSTRAINT global_id_key PRIMARY KEY (global_id)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
```
