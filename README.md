package com.agileinfoways.databasedemo.databasehandler;

import java.util.ArrayList;
import java.util.List;

import android.content.ContentValues;
import android.content.Context;
import android.database.Cursor;
import android.database.sqlite.SQLiteDatabase;
import android.database.sqlite.SQLiteOpenHelper;

import com.agileinfoways.databasedemo.model.Form_Model;

public class DatabaseHandler extends SQLiteOpenHelper {
	
	private static final int DATABASE_VERSION = 1;
	
	private static  String DATABASE_NAME = "AgileInfoways";
	private static final String TABLE1_NAME = "Table1";

	private static final String KEY_ID = "id";
	private static final String KEY_NAME1 = "name";
	private static final String KEY_NAME2 = "mobile";

	public DatabaseHandler(Context context) {
		super(context, DATABASE_NAME, null, DATABASE_VERSION);
	}

	public void onCreate(SQLiteDatabase db) {

		String CREATE_TABLE1 = "CREATE TABLE " + TABLE1_NAME + "(" + KEY_ID
				+ " INTEGER PRIMARY KEY," + KEY_NAME1 + " TEXT," + KEY_NAME2
				+ " TEXT)";

		db.execSQL(CREATE_TABLE1);
	}

	public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
		db.execSQL("DROP TABLE IF EXISTS " + TABLE1_NAME);
		onCreate(db);
	}

	public void addForm(Form_Model form_Object) {
		SQLiteDatabase database = this.getWritableDatabase();
		ContentValues values = new ContentValues();

		values.put(KEY_NAME1, form_Object.getName());
		values.put(KEY_NAME2, form_Object.getMobile());

		database.insert(TABLE1_NAME, null, values);
		database.close();

	}

	public void deleteRoute() {

		SQLiteDatabase database = this.getWritableDatabase();

		String selectQuery = "SELECT  * FROM " + TABLE1_NAME;

		Cursor cursor = database.rawQuery(selectQuery, null);

		if (cursor.moveToFirst()) {
			do {
				database.delete(TABLE1_NAME, KEY_NAME1 + "=?",
						new String[] { cursor.getString(1) });
			} while (cursor.moveToNext());

		}
		cursor.close();

		database.close();

	}

	public void updateForm(Form_Model form_Object) {

		SQLiteDatabase database = this.getWritableDatabase();
		ContentValues values = new ContentValues();

		values.put(KEY_ID, 1);
		values.put(KEY_NAME1, form_Object.getName());
		values.put(KEY_NAME2, form_Object.getMobile());

		database.update(TABLE1_NAME, values, KEY_ID + " = ?",
				new String[] { "1" });
		database.close();
	}

	public List<Form_Model> getAll_form_Object() {
		List<Form_Model> form_Object_List = new ArrayList<Form_Model>();

		String selectQuery = "SELECT  * FROM " + TABLE1_NAME;
		SQLiteDatabase db = this.getWritableDatabase();

		Cursor cursor = db.rawQuery(selectQuery, null);

		if (cursor.moveToFirst()) {
			do {
				Form_Model form_Object = new Form_Model();

				form_Object.setName(cursor.getString(1));
				form_Object.setMobile(cursor.getString(2));

				form_Object_List.add(form_Object);
			} while (cursor.moveToNext());

		}
		cursor.close();
		return form_Object_List;
	}

	public void abc() {
		int a = 0;
	}
}

