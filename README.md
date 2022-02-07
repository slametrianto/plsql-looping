# plsql-looping




PROCEDURE PROC_LIST_KAB(inNoProp NUMBER,			
						outStatus OUT PLS_INTEGER,
						outMessage OUT VARCHAR2,
						outMaster OUT VARCHAR2)
		IS
		vOutStatus			pls_integer;
		vOutJsonData		json;

		vOutResAccess		BOOLEAN;
		vJsonDetail			JSON_LIST;
		vJsonDetailValue	json;
		vJsonOut			json;
		vCountKAB			PLS_INTEGER;
	
	
		BEGIN
		outStatus	:= SIAK_APPS.PKG_SIAK_LIBS.RESP200;

		SELECT 
			COUNT(1) 
		INTO 
			vCountKAB
		FROM 
			SIAK_REFF.SETUP_KAB
		WHERE 
			NO_PROP=inNoProp;

		IF vCountKAB > 0 THEN
			vJsonDetail := JSON_LIST('[]');
			FOR recData in(
				SELECT 
				
					{do something} /melakukan sesuatu
					
				FROM 
					SIAK_REFF.SETUP_KAB
				WHERE
					NO_PROP=inNoProp
			)
			LOOP
			vJsonDetailValue	:= json('{}');
				
				
				{do something} /melakukan sesuatu
				
			vJsonDetail.append(vJsonDetailValue.to_json_value);
			END LOOP;


			outMaster		:= vJsonDetail.to_char(SIAK_APPS.PKG_SIAK_LIBS.JSON_PRETTY); -- konversi json to string
			vJsonOut		:= json('{}'); --clear json variable
		ELSE
			outStatus	:= SIAK_APPS.PKG_SIAK_LIBS.RESP404;
			outMessage	:= SIAK_APPS.PKG_SIAK_LIBS.MSG_USER_DATA_NOT_FOUND;
		END IF;

		EXCEPTION WHEN OTHERS THEN
			SIAK_APPS.PKG_ERROR_MESSAGE.PROC_ADD(sys_context('USERENV', 'CURRENT_USER')||'.'||utl_call_stack.subprogram(1)(1)||'.'||utl_call_stack.subprogram(1)(2),SQLERRM);			
			outStatus	:= SIAK_APPS.PKG_SIAK_LIBS.RESP500;
			outMessage	:= SQLERRM;
			RAISE;
	END PROC_LIST_KAB;



	
END PKG_DKI_REF_SETUP_KAB;
/
ALTER PACKAGE SIAK_REFF.PKG_DKI_REF_SETUP_KAB COMPILE BODY;
/
