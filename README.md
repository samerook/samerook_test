1. 다운로드
 - **[링크 제공](링크 제공)**
1. 설치하기
 - 간단한 설치 도움말 제공
 - **[링크로 스크린 샷 설치방법 제공](링크로 스크린 샷 설치방법 제공)** (jar 파일 복사 붙여 넣기 후, 빌드 패스 설정까지)
1. **[Cookbook](Cookbook)**
You'll find some wireloop recipes here!
1. 샘플프로젝트
 - **create**
<br> Usage example: <pre><code>DatabaseOpenHelper databaseHelper = new DatabaseOpenHelper(this);
EntityManager em = EntityManagerFactory.getEntityManager();
entityManager.createTable(My.class, SQLiteDatabaseUtils.getWritableDatabase(databaseHelper));
</code></pre>
And without any SQL code: <pre><code>@Override
        public void onCreate(SQLiteDatabase db) {
            db.execSQL("CREATE TABLE " + NOTES_TABLE_NAME + " ("
             	+ Notes._ID + " INTEGER PRIMARY KEY,"
                      + Notes.TITLE + " TEXT,"
                      + Notes.NOTE + " TEXT,"
                      + Notes.CREATED_DATE + " INTEGER,"
                      + Notes.MODIFIED_DATE + " INTEGER"
                      + ");");
        }
@Override
        public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
            Log.w(TAG, "Upgrading database from version " + oldVersion + " to "
                    + newVersion + ", which will destroy all old data");
            db.execSQL("DROP TABLE IF EXISTS notes");
            onCreate(db);
        }
</code></pre>
 - **insert / update**
<br> Usage example: <pre><code>EntityManager em = EntityManagerFactory.getEntityManager();
em.getTransaction().begin();
MyClass my = new MyClass(“something”);
em.persist(my);
em.getTransaction().commit();
</code></pre>
And without any SQL code: <pre><code>@Override
    public Uri insert(Uri uri, ContentValues initialValues) {
        // Validate the requested uri
        if (sUriMatcher.match(uri) != NOTES) {
            throw new IllegalArgumentException("Unknown URI " + uri);
        }
        ContentValues values;
        if (initialValues != null) {
            values = new ContentValues(initialValues);
        } else {
            values = new ContentValues();
        }
        Long now = Long.valueOf(System.currentTimeMillis());
        // Make sure that the fields are all set
        if (values.containsKey(NotePad.Notes.CREATED_DATE) == false) {
            values.put(NotePad.Notes.CREATED_DATE, now);
        }
        if (values.containsKey(NotePad.Notes.MODIFIED_DATE) == false) {
            values.put(NotePad.Notes.MODIFIED_DATE, now);
        }
        if (values.containsKey(NotePad.Notes.TITLE) == false) {
            Resources r = Resources.getSystem();
            values.put(NotePad.Notes.TITLE, r.getString(android.R.string.untitled));
        }
        if (values.containsKey(NotePad.Notes.NOTE) == false) {
            values.put(NotePad.Notes.NOTE, "");
        }
        SQLiteDatabase db = mOpenHelper.getWritableDatabase();
        long rowId = db.insert(NOTES_TABLE_NAME, Notes.NOTE, values);
        if (rowId > 0) {
            Uri noteUri = ContentUris.withAppendedId(NotePad.Notes.CONTENT_URI, rowId);
            getContext().getContentResolver().notifyChange(noteUri, null);
            return noteUri;
        }
        throw new SQLException("Failed to insert row into " + uri);
    }
</code></pre>
 - **delete**
<br> Usage example: <pre><code>DatabaseOpenHelper databaseHelper = new DatabaseOpenHelper(this);
EntityManager em = EntityManagerFactory.getEntityManager();
entityManager.dropTable(My.class, SQLiteDatabaseUtils.getWritableDatabase(databaseHelper));
</code></pre>
And without any SQL code: <pre><code>@Override
    public int delete(Uri uri, String where, String[] whereArgs) {
        SQLiteDatabase db = mOpenHelper.getWritableDatabase();
        int count;
        switch (sUriMatcher.match(uri)) {
        case NOTES:
            count = db.delete(NOTES_TABLE_NAME, where, whereArgs);
            break;
        case NOTE_ID:
            String noteId = uri.getPathSegments().get(1);
            count = db.delete(NOTES_TABLE_NAME, Notes._ID + "=" + noteId
                    + (!TextUtils.isEmpty(where) ? " AND (" + where + ')' : ""), whereArgs);
            break;
        default:
            throw new IllegalArgumentException("Unknown URI " + uri);
        }
        getContext().getContentResolver().notifyChange(uri, null);
        return count;
    }
</code></pre>
1. **[Troubleshooting](Troubleshooting)**
1. **[JPA 공부방](JPA 공부방)**
