# Collage
//First code working with collages. This was an assignment performed last year for my data structures class.

import java.awt.Color;

public class ArtCollage {

    // The orginal picture
    private Picture original;

    // The collage picture
    private Picture collage;

    // The collage Picture consists of collageDimension X collageDimension tiles
    private int collageDimension;

    // A tile consists of tileDimension X tileDimension pixels
    private int tileDimension;
    
    /*
     * One-argument Constructor
     * 1. set default values of collageDimension to 4 and tileDimension to 100
     * 2. initializes original with the filename image
     * 3. initializes collage as a Picture of tileDimension*collageDimension x tileDimension*collageDimension, 
     *    where each pixel is black (see all constructors for the Picture class).
     * 4. update collage to be a scaled version of original (see scaling filter on Week 9 slides)
     *
     * @param filename the image filename
     */
    public ArtCollage (String filename) {

	    // WRITE YOUR CODE HERE
        this.tileDimension = 100;
        this.collageDimension = 4;

        Picture source = new Picture(filename);
        this.original=source;
        Picture target = new Picture(400, 400);
        
        for (int tcol = 0; tcol < 400; tcol++)
        
        for (int trow = 0; trow < 400; trow++)
        {
            int scol = tcol * source.width()  / 400;
            int srow = trow * source.height() / 400;
            Color color = source.get(scol, srow);
            target.set(tcol, trow, color);
        }
        this.collage = target;

    }

    /*
     * Three-arguments Constructor
     * 1. set default values of collageDimension to cd and tileDimension to td
     * 2. initializes original with the filename image
     * 3. initializes collage as a Picture of tileDimension*collageDimension x tileDimension*collageDimension, 
     *    where each pixel is black (see all constructors for the Picture class).
     * 4. update collage to be a scaled version of original (see scaling filter on Week 9 slides)
     *
     * @param filename the image filename
     */
    public ArtCollage (String filename, int td, int cd) {

	    // WRITE YOUR CODE HERE
        this.tileDimension = td;
        this.collageDimension = cd;
        int dimension=td*cd;

        Picture source = new Picture(filename);
        this.original = source;
        Picture target = new Picture(dimension, dimension);
        
        for (int tcol = 0; tcol < dimension; tcol++)
        
        for (int trow = 0; trow < dimension; trow++)
        {
            int scol = tcol * source.width()  / dimension;
            int srow = trow * source.height() / dimension;
            Color color = source.get(scol, srow);
            target.set(tcol, trow, color);
        }
        this.collage = target;

    }

    /*
     * Returns the collageDimension instance variable
     *
     * @return collageDimension
     */
    public int getCollageDimension() {

	    // WRITE YOUR CODE HERE
        return collageDimension;
    }

    /*
     * Returns the tileDimension instance variable
     *
     * @return tileDimension
     */
    public int getTileDimension() {

	    // WRITE YOUR CODE HERE
        return tileDimension;
    }

    /*
     * Returns original instance variable
     *
     * @return original
     */
    public Picture getOriginalPicture() {

	    // WRITE YOUR CODE HERE
        return original;
    }

    /*
     * Returns collage instance variable
     *
     * @return collage
     */
    public Picture getCollagePicture() {

	    // WRITE YOUR CODE HERE
        return collage;
    }
    
    /*
     * Display the original image
     * Assumes that original has been initialized
     */
    public void showOriginalPicture() {

	    // WRITE YOUR CODE HERE
        original.show();
    }

    /*
     * Display the collage image
     * Assumes that collage has been initialized
     */
    public void showCollagePicture() {

	    // WRITE YOUR CODE HERE
        Picture pic = new Picture(collage);
        pic.show();
    }

    /*
     * Replaces the tile at collageCol,collageRow with the image from filename
     * Tile (0,0) is the upper leftmost tile
     *
     * @param filename image to replace tile
     * @param collageCol tile column
     * @param collageRow tile row
     */
    public void replaceTile (String filename,  int collageCol, int collageRow) {

	    // WRITE YOUR CODE HERE
        int targetcol = collageCol * tileDimension;
        int targetrow = collageRow * tileDimension;
        Picture pic = new Picture (collage);
        Picture pic2 = new Picture (filename);
        Picture target = new Picture (tileDimension*collageDimension, tileDimension*collageDimension);
            for(int aftc = 0; aftc < tileDimension*collageDimension; aftc++)
            {
                for (int aftr = 0; aftr < tileDimension*collageDimension; aftr++)
                {
                    Color color = pic.get(aftc,aftr);
                    target.set(aftc, aftr, color);
                        if((aftc >= targetcol) && (aftc < targetcol+tileDimension) && (aftr >= targetrow) && (aftr < targetrow + tileDimension))
                    {
                        int befc = (aftc-(tileDimension * collageCol)) * pic2.width() / (tileDimension);
                        int befr = (aftr-(tileDimension * collageRow)) * pic2.height() / (tileDimension);
                        color = pic2.get(befc, befr);
                        target.set(aftc, aftr, color);

                    }
                }
            }
        this.collage = target;
        
    }
    /*
     * Makes a collage of tiles from original Picture
     * original will have collageDimension x collageDimension tiles, each tile
     * has tileDimension X tileDimension pixels
     */
    public void makeCollage () {

	    // WRITE YOUR CODE HERE

        Picture source = new Picture(collage);
        Picture target = new Picture(tileDimension * collageDimension, tileDimension*collageDimension);
        
        for (int cols = 0; cols < tileDimension*collageDimension; cols=cols + tileDimension)
        {
            for (int rows = 0; rows < tileDimension*collageDimension; rows = rows + tileDimension)
            {
                for (int colt = 0; colt < tileDimension; colt++)
                {
                    for (int rowt = 0; rowt < tileDimension; rowt++)
                    { 
                        int scol = colt * source.width()  / tileDimension;
                        int srow = rowt * source.height() / tileDimension;
                        Color color = source.get(scol, srow);
                        target.set(colt + cols, rowt + rows, color);
                    }
                }
            }    
        }
        this.collage = target;
    }
    /*
     * Colorizes the tile at (collageCol, collageRow) with component 
     * (see CS111 Week 9 slides, the code for color separation is at the 
     *  book's website)
     *
     * @param component is either red, blue or green
     * @param collageCol tile column
     * @param collageRow tile row
     */
    public void colorizeTile (String component,  int collageCol, int collageRow) {

	    // WRITE YOUR CODE HERE
        int targetcol = collageCol * tileDimension;
        int targetRow = collageRow * tileDimension;
        Picture pic = new Picture(collage);
        Picture target = new Picture(tileDimension*collageDimension, tileDimension*collageDimension);
        for (int aftc = 0; aftc < tileDimension*collageDimension; aftc++)
        {
            for(int aftr = 0; aftr < tileDimension*collageDimension; aftr++)
            {
                Color color = pic.get(aftc, aftr);
                target.set(aftc, aftr, color);
                if ((aftc >= targetcol) && (aftc < targetcol+tileDimension) && (aftr >= targetRow) && (aftr < targetRow+tileDimension))
                {
                    if(component == "blue")
                    {
                        int a = color.getBlue();
                        target.set(aftc, aftr, new Color (0,0, a));
                    }
                    else if (component == "green")
                    {
                        int b = color.getGreen();
                        target.set(aftc, aftr, new Color(0,b,0));
                    }
                    else if (component == "red")
                    {
                        int c = color.getRed();
                        target.set(aftc, aftr, new Color(c,0,0));
                    }
                }    
            }
        }
        this.collage = target;
    }

    /*
     * Grayscale tile at (collageCol, collageRow)
     * (see CS111 Week 9 slides, the code for luminance is at the book's website)
     *
     * @param collageCol tile column
     * @param collageRow tile row
     */

    public void grayscaleTile (int collageCol, int collageRow) {

	    // WRITE YOUR CODE HERE
        int targetcol = collageCol * tileDimension;
        int targetRow = collageRow * tileDimension;
        Picture pic = new Picture(collage);
        Picture target = new Picture(tileDimension*collageDimension, tileDimension*collageDimension);
        for (int aftc = 0; aftc < tileDimension*collageDimension; aftc++)
        {
            for(int aftr = 0; aftr < tileDimension*collageDimension; aftr++)
            {
                Color color = pic.get(aftc, aftr);
                target.set(aftc, aftr, color);
                if ((aftc >= targetcol) && (aftc < targetcol+tileDimension) && (aftr >= targetRow) && (aftr < targetRow+tileDimension))
                {
                    Color gray  = Luminance.toGray(color);
                    target.set(aftc, aftr, gray);
                }
            }
        }
        this.collage = target;

    }


    /*
     *
     *  Test client: use the examples given on the assignment description to test your ArtCollage
     */

    public static void main (String[] args) {

    // Creates a collage of 3x3 tiles. 
// Each tile dimension is 200x200 pixels
ArtCollage art = 
new ArtCollage(args[0], 200, 3);

art.makeCollage();

// Replace tile at col 1, row 1 with 
// args[1] image
art.replaceTile(args[1],1,1);
art.showCollagePicture(); 
    }
}
