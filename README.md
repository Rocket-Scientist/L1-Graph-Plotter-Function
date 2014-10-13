L1-Graph-Plotter-Function
=========================
int GraphPlotter(LOBFType datatable){           /* Function plots graph in graphics window.*/
    int row, column, xres, yres, b, xnew, ynew, zerox, zeroy, yLOBFstart, yLOBFend, minxRow, maxxRow;  /* Variables declared.*/
    float x, y, xmin, xmax, ymin, ymax; 
    
    if (datatable.currentrow == 0) {         /* Displays message if there are no points to plot.*/
		    printf("The current table is empty\n"); }
	else {   
        xmin = datatable.table[0][COLX]; xmax = datatable.table[0][COLX]; ymin = datatable.table[0][COLY]; ymax = datatable.table[0][COLY]; minxRow = 0; maxxRow = 0;
        for (row=1; row<=datatable.currentrow-1; row=row+1) {
	           if (datatable.table[row][COLX]<xmin) {  xmin = datatable.table[row][COLX]; minxRow = row; }  /* The maximum and minimum values are*/
	           if (datatable.table[row][COLX]>xmax) {  xmax = datatable.table[row][COLX]; maxxRow = row; }  /* found by comparing the current value*/
	           if (datatable.table[row][COLY]<ymin) {  ymin = datatable.table[row][COLY];  }  /* with the previous one.  The first values*/
	           if (datatable.table[row][COLY]>ymax) {  ymax = datatable.table[row][COLY];  }  /* in the array are set to both.*/
        }   
        if((xmin == xmax) || (ymin == ymax)) {   /* Due to the scaling of co-odrinates (xnew, ynew), the minimum and maxmimum values can't*/
                printf("\nYou must have 2 or more unique points\n");  }   /* equal each other.*/
        else {  GrSetMode(GR_default_graphics);     /* The graphics mode is called from the library.*/
                b = 40;                             /* b is the graphics window border of 40 pixels.*/
                xres = GrScreenX();                 /* Maximum x point on the graphics window.*/
                yres = GrScreenY();                 /* Maximum y point on the graphics window.*/
                                                   
                for (row=0; row<=datatable.currentrow-1; row=row+1) {
                    x = datatable.table[row][COLX];    /* Uses the x value from the current row.*/
                    y = datatable.table[row][COLY];    /* Uses the y value from the current row.*/
                    xnew = (b/2)+(x-xmin)*((xres-b)/(xmax-xmin));       /* xnew and ynew are plotted to fill the whole graphics window.*/
                    ynew = yres -((b/2)+(y-ymin)*((yres-b)/(ymax-ymin))); 
                    GrCircle(xnew,ynew, 2, 15); }                   /* plots the points within the data structure.*/
                                                                                                            
                zerox = (b/2)+(0-xmin)*((xres-b)/(xmax-xmin));      /* These are the points that both axis lines will go through - (0,0).*/
                zeroy = yres -((b/2)+(0-ymin)*((yres-b)/(ymax-ymin)));
                y = (datatable.a + (datatable.b * datatable.table[minxRow][COLX]));
                yLOBFstart = yres -((b/2)+(y-ymin)*((yres-b)/(ymax-ymin)));
                y = (datatable.a + (datatable.b * datatable.table[maxxRow][COLX]));
                yLOBFend = yres -((b/2)+(y-ymin)*((yres-b)/(ymax-ymin)));
                
                GrLine(zerox, 0, zerox, yres, 15);   /* Vertical axis line.*/
                GrLine(0, zeroy, xres, zeroy, 15);   /* Horizontal axis line.*/
                GrLine(b/2, yLOBFstart, (xres-(b/2)), yLOBFend, 10);  /* Line of best fit - the y coordinates are calculated above.*/
                DrawText(b/2, b/2, &ylabel, GR_ALIGN_LEFT, GR_ALIGN_TOP );   DrawText(xres-(b/2), yres-(b/2), &xlabel, GR_ALIGN_RIGHT, GR_ALIGN_BOTTOM);
                DrawText(320, 0, &graphtitle, GR_ALIGN_CENTER, GR_ALIGN_TOP ); }     /* 3 commands to display the axes labels and graph title on the graphics window.*/
    }
}
