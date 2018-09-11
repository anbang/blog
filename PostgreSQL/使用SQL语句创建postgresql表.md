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
    tran_count integer,
    CONSTRAINT account_pkey PRIMARY KEY (account)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
ALTER TABLE public.accounts
    OWNER to postgres;
 
COMMENT ON COLUMN public.accounts.account
    IS '账户地址';
 
COMMENT ON COLUMN public.accounts.type
    IS '账户类型（1普通账户、2合约账户）';
 
COMMENT ON COLUMN public.accounts.balance
    IS '余额，单位是Wei';
 
COMMENT ON COLUMN public.accounts.tran_count
    IS '交易次数';
```

交易表创建

```postgresql
-- Table: public.transaction
 
-- DROP TABLE public.transaction;
 
CREATE TABLE public.transaction
(
    pkid bigserial,
    hash text COLLATE pg_catalog."default" NOT NULL,
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
    is_fork boolean,
    is_invalid boolean,
    is_fail boolean,
    is_on_mc boolean,
    mci bigint,
    latest_included_mci bigint,
    mc_timestamp bigint,
    CONSTRAINT pkid_pkey PRIMARY KEY (pkid)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
ALTER TABLE public.transaction
    OWNER to postgres;
 
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
 
COMMENT ON COLUMN public.transaction.is_fork
    IS 'is_fork';
 
COMMENT ON COLUMN public.transaction.is_invalid
    IS 'is_invalid';
 
COMMENT ON COLUMN public.transaction.is_fail
    IS 'is_fail';
 
COMMENT ON COLUMN public.transaction.is_on_mc
    IS 'is_on_mc';
 
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
    CONSTRAINT parents_id_pkey PRIMARY KEY (parents_id),
    CONSTRAINT parents_item_parent_key UNIQUE (item, parent)
)
WITH (
    OIDS = FALSE
)
TABLESPACE pg_default;
 
ALTER TABLE public.parents
    OWNER to postgres;
 
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
 
ALTER TABLE public.witness
OWNER to postgres;
 
COMMENT ON COLUMN public.witness.item
IS '元素';
 
COMMENT ON COLUMN public.witness.account
IS 'account';
```

