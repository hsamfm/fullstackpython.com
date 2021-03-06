title: sqlalchemy.dialects.postgresql BIT code examples
category: page
slug: sqlalchemy-dialects-postgresql-bit-examples
sortorder: 500031007
toc: False
sidebartitle: sqlalchemy.dialects.postgresql BIT
meta: Python example code for the BIT class from the sqlalchemy.dialects.postgresql module of the SQLAlchemy project.


BIT is a class within the sqlalchemy.dialects.postgresql module of the SQLAlchemy project.


## Example 1 from sqlalchemy-utils
[sqlalchemy-utils](https://github.com/kvesteri/sqlalchemy-utils)
([project documentation](https://sqlalchemy-utils.readthedocs.io/en/latest/)
and
[PyPI package information](https://pypi.org/project/SQLAlchemy-Utils/))
is a code library with various helper functions and new data types
that make it easier to use [SQLAlchemy](/sqlalchemy.html) when building
projects that involve more specific storage requirements such as
[currency](https://sqlalchemy-utils.readthedocs.io/en/latest/data_types.html#module-sqlalchemy_utils.types.currency).
The wide array of
[data types](https://sqlalchemy-utils.readthedocs.io/en/latest/data_types.html)
includes [ranged values](https://sqlalchemy-utils.readthedocs.io/en/latest/range_data_types.html)
and [aggregated attributes](https://sqlalchemy-utils.readthedocs.io/en/latest/aggregates.html).

[**sqlalchemy-utils / sqlalchemy_utils / types / bit.py**](https://github.com/kvesteri/sqlalchemy-utils/blob/master/sqlalchemy_utils/types/bit.py)

```python
# bit.py
import sqlalchemy as sa
~~from sqlalchemy.dialects.postgresql import BIT


class BitType(sa.types.TypeDecorator):
    impl = sa.types.BINARY

    def __init__(self, length=1, **kwargs):
        self.length = length
        sa.types.TypeDecorator.__init__(self, **kwargs)

    def load_dialect_impl(self, dialect):
        if dialect.name == 'postgresql':
            return dialect.type_descriptor(BIT(self.length))
        elif dialect.name == 'sqlite':
            return dialect.type_descriptor(sa.String(self.length))
        else:
            return dialect.type_descriptor(type(self.impl)(self.length))



## ... source file continues with no further BIT examples...

```

