DartDB
======

DartDB is a in-memory relational database for Dart.

With DartDB, you can create table and manipulate table data in a similar maner as you would do in SQL.

DartDB is in development and cannot be yet used.


Example:
--------

    var countries=new Table.fromList( [
            new Column.fromList("ID",[1,2,3,4,5,6,7]),
            new Column.fromList("NAME",["France","USA","UK","Burkina-Faso","Ivory Coast","Belgium","Sénégal"])]
            
        );
        
    var cities=new Table.fromList(
            [
                 new Column.fromList("ID",[1,2,3,4,5,6,7]),
                 new Column.fromList("NAME",["Paris","Ouagadougou","New-York","Abidjan","London","Bruxel","Cannes"]),
                 new Column.fromList("COUNTRY_ID",[1,4,2,5,3,6,1])
            ]
        
        );
        
    var monuments=new Table.fromList( [
                                   new Column.fromList("ID",[1,2]),
                                   new Column.fromList("NAME",["Eifel Tower","Empire State Building"]),
                                   new Column.fromList("CITY_ID",[1,3])
                                   ]
        
        ); 
    var it=countries.joinWith(
                  cities.joinWith(monuments,mode: FULL_JOIN, matcher: (a) => a[0]==a[5] ),
                  mode: LEFT_JOIN, matcher: (a) => a[0]==a[4] )
               .where( (e) => (e[0]!=null && e[1] != "France"))
               .orderBy([1]).select([2,4,7])
               ;
    var r=it.toList(growable:false);
    expect (r, equals([
                         ['Belgium', null, null],
                         ['Burkina-Faso', null, null],
                         ['Ivory Coast', null, null],
                         ['Sénégal', null, null],
                         ['UK', null, null],
                         ['USA', 'New-York', 'Empire State Building']
                         ]));
                         
